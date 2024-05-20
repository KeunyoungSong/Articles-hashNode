---
title: "[OpenSource]Image Picker Android library 배포하기(3)"
datePublished: Mon May 20 2024 07:25:02 GMT+0000 (Coordinated Universal Time)
cuid: clwen5nzm000x09l45a3veliq
slug: opensourceimage-picker-android-library-3
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716189865939/a97fbfe2-f699-41cd-ac8e-6c5e52fc9d77.png
tags: android, fragment, builder-pattern

---

[이전 포스팅(Image Picker Android library 배포하기(2))](https://hashnode.com/post/clwedin35000a0ajydt6h9879)에서 유저에게 어떤 api 를 제공할지와 그 세부적인 기능에 대해서 다뤄보았다.

이번 포스팅에선 Fragment 의 구현과 라이브러리를 사용하기 위한 config 를 어떤 형태로 제공하도록 구현했는지 그리고 배포 과정을 살펴보겠다.

```kotlin
class SelectSaveImagePicker() : BottomSheetDialogFragment() {
	
	private var _binding: FragmentSelectSaveImagePickerBinding? = null
	private val binding get() = _binding!!
	
	private val viewModel: SelectSaveImagePickerViewModel by activityViewModels {
		ViewModelFactory(
			maxSelection = config.maxSelection, repository = ImageRepository(requireContext())
		)
	}
	
	private lateinit var imagePickerAdapter: ImagePickerAdapter
	private lateinit var permissionRequester: PermissionRequester
	
	private var spanCount: Int = 3
	private lateinit var config: PickerConfig
	private var listener: OnSelectionCompleteListener? = null
	
	interface OnSelectionCompleteListener {
		fun onSelectionComplete(selectedImages: List<String>)
	}
	
	companion object {
		private const val ARG_CONFIG = "picker_config"
		
		fun newInstance(config: PickerConfig): SelectSaveImagePicker {
			return SelectSaveImagePicker().apply {
				arguments = Bundle().apply {
					putParcelable(
						ARG_CONFIG, config
					)
				}
			}
		}
	}
	
	override fun onCreateView(
		inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?
	): View {
		_binding = FragmentSelectSaveImagePickerBinding.inflate(
			inflater, container, false
		)
		return binding.root
	}
	
	override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
		super.onViewCreated(view, savedInstanceState)
		
		config = arguments?.getParcelable(ARG_CONFIG)!!
		spanCount = if (isLandscape()) 5 else 3
		
		
		setupPermissionHandling()
		initView()
		initListener()
		initViewModel()
	}
	
	override fun onResume() {
		super.onResume()
		checkPermissionsAndUpdateVisibility()
		listener = activity as? OnSelectionCompleteListener
		
	}
	
	override fun onDestroyView() {
		super.onDestroyView()
		_binding = null
	}
	
	private fun initViewModel() {
		lifecycleScope.launch {
			viewModel.images.collect { images ->
				imagePickerAdapter.submitList(images.toList())
			}
		}
		lifecycleScope.launch {
			viewModel.selectedImagesCount.collect { count ->
				updateAddButton(count)
			}
		}
		lifecycleScope.launch {
			viewModel.event.collect { event ->
				onEvent(event)
			}
		}
	}
	
	private fun onEvent(event: ImagePickerEvent) {
		when (event) {
			ImagePickerEvent.ReachedMaxSelection -> showMaxSelectionDialog()
			else -> Unit
		}
	}
	
	private fun updateAddButton(count: Int) {
		val isEnabled = count != 0
		val color = ContextCompat.getColor(
			requireContext(), if (isEnabled) R.color.button_enabled else R.color.button_disabled
		)
		binding.tvAddButtonText.setTextColor(color)
		
		binding.tvAddCount.apply {
			isVisible = count != 0
			text = if (isEnabled) count.toString() else ""
		}
	}
	
	private fun initView() {
		setupImagePickerAdapter()
		binding.tvHandlebarDescription.text = config.descriptionText
		binding.tvAddCount.setTextColor(config.themeColor)
	}
	
	private fun setupImagePickerAdapter() {
		imagePickerAdapter = ImagePickerAdapter(config) { image ->
			viewModel.handleEvent(ImagePickerEvent.ToggleImage(image))
		}
		
		binding.rvImages.apply {
			layoutManager = GridLayoutManager(context, spanCount)
			addItemDecoration(GridSpacingItemDecoration(spanCount, config.itemSpacing))
			adapter = imagePickerAdapter
			setItemViewCacheSize(config.itemViewCacheSize)
			setHasFixedSize(true)
			itemAnimator = null
		}
	}
	
	private fun initListener() {
		binding.btnRequestPermission.setOnClickListener {
			permissionRequester.requestPermission()
		}
		binding.layoutImagesSwipeRefresh.apply {
			isEnabled = false
			setOnRefreshListener {
				viewModel.loadImages()
				isRefreshing = false
			}
		}
		binding.tvAddButton.setOnClickListener {
			listener?.onSelectionComplete(viewModel.selectedImage)
			if (config.clearSelectionOnComplete) clearSelectedImages()
			dismiss()
		}
		
		binding.tvCloseButton.setOnClickListener {
			dismiss()
		}
	}
	
	private fun setupPermissionHandling() {
		val permission = getRequiredPermission()
		
		permissionRequester = PermissionRequester(
			this, permission
		)
		permissionRequester.onPermissionChecked = {
			updateViewVisibility(
				ContextCompat.checkSelfPermission(
					requireContext(), permission
				) == PackageManager.PERMISSION_GRANTED
			)
		}
	}
	
	private fun updateViewVisibility(isPermissionGranted: Boolean) {
		binding.layoutPermissionRequest.isVisible = !isPermissionGranted
		binding.layoutImagesSwipeRefresh.isVisible = isPermissionGranted
		binding.layoutCloseAddContainer.isVisible = isPermissionGranted
		if (isPermissionGranted) viewModel.handleEvent(ImagePickerEvent.PermissionGranted)
	}
	
	private fun checkPermissionsAndUpdateVisibility() {
		val permission = getRequiredPermission()
		updateViewVisibility(
			ContextCompat.checkSelfPermission(
				requireContext(), permission
			) == PackageManager.PERMISSION_GRANTED
		)
	}
	
	private fun getRequiredPermission(): String {
		return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
			Manifest.permission.READ_MEDIA_IMAGES
		} else {
			Manifest.permission.READ_EXTERNAL_STORAGE
		}
	}
	
	private fun showMaxSelectionDialog() {
		AlertDialog.Builder(requireContext(), R.style.AlertDialog)
			.setMessage(R.string.maximum_image_selection_reached)
			.setPositiveButton(android.R.string.ok) { dialog, _ -> dialog.dismiss() }
			.create()
			.apply {
				show()
				getButton(AlertDialog.BUTTON_POSITIVE).setTextColor(resources.getColor(R.color.default_black))
			}
	}
	
	private fun isLandscape(): Boolean {
		return resources.configuration.orientation == Configuration.ORIENTATION_LANDSCAPE
	}
	
	private fun clearSelectedImages() {
		viewModel.clearSelectedImages()
	}
	
}
```

위 코드를 들여다보면...

companion object 내에 newInstance 메서드를 통해 바텀 시트를 호출하는 기능을 구현했다. 전부터 이런 구현을 많이 봐왔지만 그 맥락에서 이런 구현 방식의 필요성을 느끼진 못했는데 적어도 이번 구현에는 이런 구현방식이 유효했다.

### Fragment 는 기본 생성자가 필요하다

라이브러리 구현 과정에서는 제공할 config 를 default value 로 제공해 세팅하여 동작을 테스트했다.

하지만 fragment 구현을 끝내고 외부에서 config 를 인자로 전달해 Image Picker 를 호출한 뒤 화면 전환을 하니 에러가 발생했다.

> Caused by: [androidx.fragment.app](http://androidx.fragment.app).Fragment$InstantiationException: Unable to instantiate fragment com.opensource.bottomsheet.BottomSheetDialog: **could not find Fragment constructor**

[구글의 Fragment 문서](https://developer.android.com/reference/android/app/Fragment)를 확인해 보니 모든 프래그먼트를 상속받는 클래스는 기본 생성자를 제공해야 한다는 내용을 찾을 수 있었다.

> <mark>All subclasses of Fragment must include a public no-argument constructor.</mark> The framework will often re-instantiate a fragment class when needed, in particular during state restore, and needs to be able to find this constructor to instantiate it. If the no-argument constructor is not available, a runtime exception will occur in some cases during state restore.

이전에는 모든 설정을 클래스의 기본값으로 선언해 설정한 것이 내부적으로 기본 생성자를 생성하게 한 까닭이었다.

앞서 언급한 대로 자바는 기본값을 매개변수에 지정할 수 없고 기본값이 있는 생성자에 대해 두개의 생성자가 생긴다.

위 내용을 확인하기 위해서 코드를 열어봤다. 아래 코틀린 코드로 작업했다.

```kotlin
class BottomSheetDialog(
    val test: Int = 3
) : BottomSheetDialogFragment() {...}
```

아래는 클래스 파일로부터 IntelliJ API Decompiler 를 통해 생성된 stup source이다.

```kotlin
public final class BottomSheetDialog public constructor(test: kotlin.Int = COMPILED_CODE) : com.google.android.material.bottomsheet.BottomSheetDialogFragment {
    private final var _binding: com.opensource.bottomsheet.databinding.FragmentBottomSheetDialogBinding? /* compiled code */

    private final val binding: com.opensource.bottomsheet.databinding.FragmentBottomSheetDialogBinding /* compiled code */
        private final get

    public final val test: kotlin.Int /* compiled code */

    public open fun onCreateView(inflater: android.view.LayoutInflater, container: android.view.ViewGroup?, savedInstanceState: android.os.Bundle?): android.view.View? { /* compiled code */ }

    public open fun onDestroyView(): kotlin.Unit { /* compiled code */ }

    public open fun onViewCreated(view: android.view.View, savedInstanceState: android.os.Bundle?): kotlin.Unit { /* compiled code */ }
}
```

스텁 소스에서는 두개의 생성자가 구현된 것을 확인 할 수 없었다. 스텁코드는 디컴파일러가 클래스의 구조를 이해하기 위해 사용하는 간략화된 코드라고 하니 그 내용이 생략됬나보다.

그럼 컴파일 된 코틀린 코드를 decompile 해서 자바 코드를 살펴보자.

```kotlin
public final class BottomSheetDialog extends BottomSheetDialogFragment {
   private FragmentBottomSheetDialogBinding _binding;
   private final int test;
    
...
   public BottomSheetDialog(int test) {
      this.test = test;
   }

   // $FF: synthetic method
   public BottomSheetDialog(int var1, int var2, DefaultConstructorMarker var3) {
      if ((var2 & 1) != 0) {
         var1 = 3;
      }

      this(var1);
   }

   public BottomSheetDialog() {
      this(0, 1, (DefaultConstructorMarker)null);
   }
}
```

디컴파일된 코드에서는 test 를 인자로 받는 생성자와 인자가 없는 기본 생성자를 확인할 수 있었다.

위 같은 내부 구현 때문에 개발동안에는 나지 않던 문제가 config 를 설정하는 과정에서 발생하게됐다.

### 그럼 config 설정은 어떻게 하지...?

어찌됐던 개발자가 커스텀할 수 있는 api 를 제공하려면 어떻게든 라이브러리에 정보를 제공해야했다.

그래서 [구글의 Fragment 문서](https://developer.android.com/reference/kotlin/androidx/fragment/app/Fragment#Fragment())를 참고해 구현했다.

> Constructor used by the default FragmentFactory. You must set a custom FragmentFactory if you want to use a non-default constructor to ensure that your constructor is called when the fragment is re-instantiated.
> 
> It is strongly recommended to supply arguments with setArguments and later retrieved by the Fragment with getArguments. <mark>These arguments are automatically saved and restored alongside the Fragment.</mark>

이런 생성 방법으로 처음 세팅한 설정값을 재생성 뒤에도 유지할 수 있다고 한다.

```kotlin
companion object {
    private const val ARG_CONFIG = "picker_config"
    
    fun newInstance(config: PickerConfig): SelectSaveImagePicker {
       return SelectSaveImagePicker().apply {
          arguments = Bundle().apply {
             putParcelable(
                ARG_CONFIG, config
             )
          }
       }
    }
}

...
	override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
		super.onViewCreated(view, savedInstanceState)
		
		config = arguments?.getParcelable(ARG_CONFIG)!!
...
	}
```

### SelectSaveImagePicker 에는 단순 설정 값 외에도 선택된 이미지 결과를 반환하는 콜백함수 매개변수가 더 있었다..

Config는 data class 이기 때문에 번들에 넣어 데이터를 복원할 수 있었지만 콜백 함수는 어떻게 저장한단 말인가?

번들에 콜백을 저장할 수는 없을 것 같아 인터페이스를 구현하기로 했다.

사용처에서는 리스너를 구현하고 Image Picker 에서는 이를 resume 단계에서 리스너로 받아 적용시킨다.

```kotlin
	override fun onResume() {
		super.onResume()
		checkPermissionsAndUpdateVisibility()
		listener = activity as? OnSelectionCompleteListener
	}
```

### 이제 진짜 끝..? 아니 이제 디테일을 손보자

이제 굵직한 문제는 다 털어냈다. 설정 복원도되고 화면회전에도 앱이 깨지지 않는다. 뷰모델에 저장한 상태에 따라 뷰를 업데이트 하도록 하면된다.

가로 모드일 때는 GridLayout 의 span 을 동적으로 변경해 더 많은 아이템을 하나의 열에서 볼 수 있도록 했다.

RecyclerView 가 관리하는 holder 의 개수를 화면에 필요한 개수보다 큰 수로 동적으로 세팅할 수 있게 했다.

### 사용은 어떻게 하는데?

PickerConfig 데이터 클래스를 빌더 패턴으로 초기화해 Image Picker 에 설정할 수 있도록 했다.

```kotlin
@Parcelize
class PickerConfig(
	val itemSpacing: Int,
	@ColorInt val indicatorNumberColor: Int,
	val itemStrokeWidth: Int,
	@ColorInt val themeColor: Int,
	val descriptionText: String,
	val maxSelection: Int,
	val clearSelectionOnComplete: Boolean,
	val itemViewCacheSize: Int,
	val thumbnailScale: Float
) : Parcelable {
	class Builder(context: Context) {
		private var itemSpacing: Int = context.resources.getDimensionPixelSize(R.dimen.item_spacing)
		private var indicatorNumberColor: Int = ContextCompat.getColor(context, R.color.indicator_text)
		private var itemStrokeWidth: Int = 6
		private var themeColor: Int = ContextCompat.getColor(context, R.color.themeColor)
		private var descriptionText: String = context.resources.getString(R.string.description_bottom_sheet)
		private var maxSelection: Int = 5
		private var clearSelectionOnComplete: Boolean = false
		private var itemViewCacheSize: Int = 30
		private var thumbnailScale: Float = 0.5f
		
		fun setItemSpacing(itemSpacing: Int) = apply { this.itemSpacing = itemSpacing }
		fun setIndicatorNumberColor(@ColorInt indicatorNumberColor: Int) = apply { this.indicatorNumberColor = indicatorNumberColor }
		fun setIndicatorNumberColorHex(colorString: String) = apply { this.indicatorNumberColor = parseColor(colorString) }
		fun setItemStrokeWidth(itemStrokeWidth: Int) = apply { this.itemStrokeWidth = itemStrokeWidth }
		fun setThemeColor(@ColorInt themeColor: Int) = apply { this.themeColor = themeColor }
		fun setThemeColorHex(colorString: String) = apply { this.themeColor = parseColor(colorString) }
		fun setDescriptionText(descriptionText: String) = apply { this.descriptionText = descriptionText }
		fun setMaxSelection(maxSelection: Int) = apply { this.maxSelection = maxSelection }
		fun setClearSelectionOnComplete(clearSelectionOnComplete: Boolean) = apply { this.clearSelectionOnComplete = clearSelectionOnComplete }
		fun setItemViewCacheSize(itemViewCacheSize: Int) = apply { this.itemViewCacheSize = itemViewCacheSize }
		fun setThumbnailScale(thumbnailScale: Float) = apply { this.thumbnailScale = thumbnailScale }
		
		fun build(): PickerConfig {
			return PickerConfig(
				itemSpacing = itemSpacing,
				indicatorNumberColor = indicatorNumberColor,
				itemStrokeWidth = itemStrokeWidth,
				themeColor = themeColor,
				descriptionText = descriptionText,
				maxSelection = maxSelection,
				clearSelectionOnComplete = clearSelectionOnComplete,
				itemViewCacheSize = itemViewCacheSize,
				thumbnailScale = thumbnailScale
			)
		}
		
		private fun parseColor(colorString: String): Int {
			return android.graphics.Color.parseColor(colorString)
		}
	}
}
```

* GridLayout 의 아이템 간격
    
* 인디케이터 숫자 색상
    
* 선택된 아이템의 경계선 두께
    
* 테마 색상
    
* BottomSheet 의 handler 및 설명
    
* 가능한 최대 선택 개수
    
* 이미지 선택과 동시에 저장된 상태를 초기화 할 것인지 여부
    
* RecyclerView 의 holder 개수
    
* Item 에 띄워질 썸네일의 해상도 스케일
    

색상은 color 리소스와 Hex 두 가지 방법으로 구현했다.

### 이제 Activity 에서 호출해 사용해보자

```kotlin
class MainActivity : AppCompatActivity(), SelectSaveImagePicker.OnSelectionCompleteListener {
	
	private lateinit var binding: ActivityMainBinding
	
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		binding = ActivityMainBinding.inflate(layoutInflater)
		setContentView(binding.root)
		binding.button.setOnClickListener {
			val customPickerConfig = PickerConfig.Builder(this)
				.setMaxSelection(5)
				.setThemeColor(ContextCompat.getColor(this, color.themeColor))
				.setItemSpacing(12)
				.setIndicatorNumberColorHex("#FFFFFF")
				.setItemStrokeWidth(4)
				.setDescriptionText("Please select images")
				.setClearSelectionOnComplete(true)
				.setItemViewCacheSize(30)
				.setThumbnailScale(0.8f)
				.build()
			
			val imagePicker = SelectSaveImagePicker.newInstance(customPickerConfig)
			imagePicker.show(supportFragmentManager, "IMAGE_PICKER")
		}
	}
	
	override fun onSelectionComplete(selectedImages: List<String>) {
		Log.d("Picker", "Selected images: $selectedImages")
		// 선택된 이미지를 처리하는 로직을 여기에 추가합니다.
	}
}
```

빌더 패턴으로 구현된 Config를 초기화하고 리스너를 구현한 뒤 화면에 띄우면 된다!

각각의 설정값들은 일일히 설정되지 않더라도 기본값이 설정되어 있어 작동에 문제가 없다.

---

배포 과정은 다음 포스팅에 작성하겠다.