---
title: "Image Picker Android library 배포하기(2)"
datePublished: Mon May 20 2024 02:55:11 GMT+0000 (Coordinated Universal Time)
cuid: clwedin35000a0ajydt6h9879
slug: image-picker-android-library-2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716173705852/bc85edc1-a6ae-48da-92f1-e4222b64eb1b.png

---

[이전 글(Image Picker Android library 배포하기(1))](https://hashnode.com/post/clwdjildq000c08k02lztfeyc) 에서 왜 Custom Image Picker 를 만들생각을 했는지와 기본적인 UX/UI 구현간에 있었던 어려움을 정리했다.

이번 글에선 실제 기능 구현에서 있던 크고 작은 고민들과 배포를 하기까지의 과정을 담아보겠다.

### 라이브러리를 개발하기 위한 나름 구체적인? 계획하기

여태 팀 프로젝트를 하며 항상 작성해왔던 요구사항 정의서를 간략히 적어봤다.

**기능적 요구사항**

1. 기본 기능
    
    * 이미지 다중 선택
        
    * 이미지 개수 제한
        
    * 길게 클릭 시, 이미지 미리보기
        
2. 고급 기능
    
    * 선택된 이미지 상태 복원\*
        
    * 선택된 순서 넘버링
        
3. 사용자 인터페이스
    
    * Bottom sheet dialog 사용으로 사용성 개선
        
    * 인터페이스 테마 변경 가능
        
    * 간편한 사용자 경험을 위한 직관적 디자인
        
    * 개발자가 사용하기 쉬운 api 제공
        

비기능적 요구사항

1. 성능 요구사항
    
    * 큰 이미지 목록을 빠르고 효율적으로 로드할 수 있어야 한다
        
        * 이미지 목록의 썸네일을 실제 크기보다 작은 해상도로 로드한다면?
            
        * RecyclerView 가 사용하는 holder 의 개수를 화면에 필요한 양보다 더 많이 사용하도록 한다면?
            
        * 동적으로 view 를 그려 상태를 변경하지 않고 vector 이미지로 그린 이미지를 최대한 사용한다면?
            
2. 호환성 요구사항
    
    * API 28 이상을 지원해야 한다
        
    * 가로모드를 지원해야 한다
        

여러 요구사항 중 단연 가장 중요한 우선순위는 Image Picker 에서 상태를 관리해야 한다는 것이었다.

래퍼런스로 참고하고 있는 Google 의 PhotoPicker 를 다시 한번 보면 바텀 시트 내에 여러 이미지 썸네일들이 있고 그 썸네일 안에는 이미지가 선택됐는지 여부를 알 수 있게하는 Indicator(체크박스?)가 있다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716117524465/06f8ec6f-5ffd-4d2e-81f3-ec7c6933b22b.png align="center")

RecyclerView 의 아이템으로 들어가게 될 뷰의 상태 변화에 따라 많은 뷰들의 상태를 관리해야했다.

### Adapter 내에 이런저런 item 에 대한 상태변경 로직을 두긴 싫다...

이런 생각에 item 을 Custom View 로 만들 계획을 하였고 우선 Item 에서 관리해야 하는 상태를 생각해봤다.

아이템 선택 여부, 인디케이터 숫자, 인디케이터 내 넘버링 색상, 선택 시 생기는 storke 굵기, 테마 컬러, 썸네일 스케일을 생각해 볼 수 있었다.

### 그럼 커스텀 뷰를 만들어보자

[구글의 뷰 클래스 만들기 문서](https://developer.android.com/develop/ui/views/layout/custom-views/create-view?hl=ko)를 보면 몇 가지 스탭을 통해 구현할 수 있었다.

1. Subclass a view(뷰 서브 클래스 만들기)
    
2. Define custom attributes(커스텀 속성 정의)
    
3. Apply custom attributes(커스텀 속성 적용)
    
4. Add properties and events(속성과 이벤트 추가)
    

우선 어떤 뷰를 상속해 item view 를 구현할지를 생각해야 했다. 처음엔 이미지를 보여줘야 하니까 ImageView를 사용하면되는 거 아니야? 라고 생각했다. <s>(사실 그래서 처음에 그렇게 구현했다.)</s>

하지만 item 의 기능을 보면 이미지로드 뿐 아니라 상태 변경에 따라 다른 여러 뷰들의 변화가 같이 일어나는 것을 알 수 있었고 ConstraintLayout 을 상속하는 커스텀 뷰를 만들기로 했다.

문서에 따르면

> All the view classes defined in the Android framework extend [`View`](https://developer.android.com/reference/android/view/View). Your custom view can also extend `View` directly, or you can save time by extending one of the existing view subclasses, such as [`Button`](https://developer.android.com/reference/android/widget/Button).
> 
> To allow Android Studio to interact with your view, at a minimum you must provide a constructor that takes a [`Context`](https://developer.android.com/reference/android/content/Context) and an [`AttributeSet`](https://developer.android.com/reference/android/util/AttributeSet) object as parameters. This constructor allows the layout editor to create and edit an instance of your view.

안드로이드 프레임워크의 모든 뷰 클래스는 View 를 확장한다. 시간을 좀 절약 하려면 이미 존재하는 뷰를 확장해 사용하면된다. **안드로이드 스튜디오에서 작성한 커스텀뷰를 사용하고 싶다면 Context 와 AttributeSet 은 기본적으로 제공해야한다.**

라고 한다. 이후 몇몇 래퍼런스를 참고했고 아래와 같은 클래스를 만들 수 있었다

그럼 아래 코드를 들여다보자

* @JvmOverloads
    
* 생성자의 인자들
    

```kotlin
class ImagePickerItemView @JvmOverloads constructor(
	context: Context,
	attrs: AttributeSet? = null,
	defStyleAttr: Int = 0
) : ConstraintLayout(
	context,
	attrs,
	defStyleAttr
) { ... }
```

가장 위에 `@JvmOverLoads` 키워드를 볼 수 있다. 해당 키워드는 **코틀린에서 함수의 인자에 기본값을 지정할 수 있지만 자바에서는 그럴 수 없기 때문에 필요한 애노테이션**이다.

해당 키워드를 사용하면 코틀린 코드를 자바 코드로 변환할 때 제일 첫 번째 인자부터 하나씩 다음 인자를 포함하는 생성자를 모두 생성해준다. 초기화 값이 있는 생성자는 자바 코드에서 내부적으로 초기화 값을 세팅하여 생성자를 호출한다.

다음으로 생성자가 갖는 인자를 보자. context, attrs, (defStyleAttr) 은 앞서 문서에서 봤듯 View 가 안드로이드 스튜디오와 상호작용하기 위해서 필요한 인자들이다.

context 는 ImagePickerItemView 가 속한 당시의 context 를 받는다

attrs 는 xml 레이아웃 파일에 정의된 속성들을 전달한다. 이를 통해 커스텀 뷰가 어떤 속성 값을 받을 수 있는 지 정의한다.

defStyleAttr 은 뷰에 적용할 기본 스타일 속성의 리소스 ID를 나타낸다. styles.xml 리소스에 특정 스타일을 미리 지정해두고 그 스타일을 사용해 기본값을 정할 수 있다.

나도 처음에 attrs 값을 선언해 xml 을 통해서도 값을 설정할 수 있게 커스텀 뷰를 구현했다. 하지만 item view 를 만들고 있고 모든 속성들은 adapter 에서 관리될 것이기 때문에 attrs 관련 로직을 모두 없애고 속성을 세팅하는 기능만 남겼다.

이렇게 item view 가 갖는 상태들중 일부는 나중에 개발자가 세팅할 수 있는 옵션으로 제공했다.

### Item view 를 만들었으니 화면에 보일 이미지 목록을 가져와보자

로컬의 이미지 경로를 가져오는 로직은 아래와 같이 사용했다.

```kotlin
class ImageRepository(private val context: Context) {
	
	fun loadImages(): Flow<List<Image>> = flow {
		val images = mutableListOf<Image>()
		val projection = arrayOf(MediaStore.Images.Media._ID)
		val sortOrder = "${MediaStore.Images.Media.DATE_ADDED} DESC"
		
		val cursor = context.contentResolver.query(
			MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
			projection,
			null,
			null,
			sortOrder
		)
		
		cursor?.use {
			val idColumn = cursor.getColumnIndexOrThrow(MediaStore.Images.Media._ID)
			while (cursor.moveToNext()) {
				val id = cursor.getLong(idColumn)
				val contentUri: Uri = Uri.withAppendedPath(
					MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
					id.toString()
				)
				images.add(
					Image(
						contentUri.toString(),
						false
					)
				)
			}
		}
		emit(images)  // Emit the list of images as a single batch
	}.flowOn(Dispatchers.IO)  // Perform operations on the I/O dispatcher
}
```

사실 단순 데이터 로드이기 때문에 Flow로 처리할 이유는 없다고 생각했지만 최근 Flow 에 대한 강의를 완강했고 학습한 내용도 깃헙에 정리하고 있기 때문에 사용해봤다.

다른 글에서 Flow 에 대해 학습한 내용도 정리하겠다.

### 각 이미지는 어떻게 처리하지?

```kotlin
data class Image(
	val uri: String,
	val isSelected: Boolean = false,
	val selectionOrder: Int = -1
)
```

우선 일반적으로 갤러리엔 수천~수만개의 이미지가 있고 이 로컬 경로와 필요한 상태를 저장해야하니 최대한 데이터의 사이즈를 줄이고 싶었다.

그래서 **널러블한 타입을 쓰지 않고 자바 코드로 변환될 때 primitive type 으로 매핑되도록 했다.** selectionOrder 은 이미지가 선택된 순서를 나타내는데 -1 을 기본값으로 null 로 간주하였다.

### 이제 Adapter 를 구현해보자

데이터 변경을 자동으로 처리해주고 필요한 로직이 단순하다고 생각해 RecyclerView 를 확장한 ListAdapter 를 사용했다.

커스텀 뷰로 item view 를 구현한 덕에 adapter 내에서 코드는 간결하게 작성되었다. 아래 코드에서 보이는 config 는 개발자가 커스텀해 라이브러리 호출 시 세팅하는 값이다.

```kotlin
class ImagePickerAdapter(
	private val pickerConfig: PickerConfig,
	private val onImageSelected: (Image) -> Unit
) : ListAdapter<Image, ImagePickerAdapter.ImageViewHolder>(DiffCallback()) {
	
	inner class ImageViewHolder(private val binding: ItemImageBinding) :
		RecyclerView.ViewHolder(binding.root) {
		
		fun bind(image: Image) {
			binding.imagePickerItem.setOnClickListener {
				onImageSelected(image)
			}
			binding.imagePickerItem.apply {
				isSelected = image.isSelected
				setIndicatorNumber(image.selectionOrder)
				setIndicatorNumberColor(pickerConfig.indicatorNumberColor)
				setThemeColor(pickerConfig.themeColor)
				setItemStrokeWidth(pickerConfig.itemStrokeWidth)
				loadImage(image.uri)
				setThumbnailScale(pickerConfig.thumbnailScale)
			}
		}
	}
	
	override fun onCreateViewHolder(
		parent: ViewGroup,
		viewType: Int
	): ImageViewHolder {
		return ImageViewHolder(
			ItemImageBinding.inflate(
				LayoutInflater.from(parent.context),
				parent,
				false
			)
		)
	}
	
	override fun onBindViewHolder(
		holder: ImageViewHolder,
		position: Int
	) {
		holder.bind(getItem(position))
	}
	
	class DiffCallback : DiffUtil.ItemCallback<Image>() {
		
		override fun areItemsTheSame(
			oldItem: Image,
			newItem: Image
		): Boolean = oldItem.uri == newItem.uri
		
		override fun areContentsTheSame(
			oldItem: Image,
			newItem: Image
		): Boolean =
			oldItem.isSelected == newItem.isSelected && oldItem.selectionOrder == newItem.selectionOrder
	}
}
```

### 로컬에서 이미지를 가져오기 위한 권한을 처리하자

API 28 이상에 대한 지원을 계획했기 때문에 `TIRAMISU` 이후 부터 변경된 권한 요청에 대해 대응해야 했다.

이미지 경로를 가져오기 위해 `TIRAMISU` 이상에선 READ\_MEDIA\_IMAGES 권한을 요청해야 하고 그 이전엔 READ\_EXTERNAL\_STORAGE 권한을 사용했기 때문에 그렇게 구현했다.

이전부터 권한 요청 관련 로직이 화면에 있는 것과 registerForActivityResult 를 통한 권한요청 방식이 세련되지 못하다고 생각했기에 권한 요청에 대한 로직들을 PermissionRequester class 를 만들어 외부로 빼줬다.

전체 로직은 이렇다.

Fragment 에서 어떤 로직을 요청하고 있는지 확인하고 싶었기 때문에 필요한 권한을 가져오는 함수를 만들었다.

```kotlin
private fun getRequiredPermission(): String {
    return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
       Manifest.permission.READ_MEDIA_IMAGES
    } else {
       Manifest.permission.READ_EXTERNAL_STORAGE
    }
}
```

이후 가져온 권한을 PermissionRequester 에 보낸다.

```kotlin
private lateinit var permissionRequester: PermissionRequester
...
private fun setupPermissionHandling() {
	val permission = getRequiredPermission()
	permissionRequester = PermissionRequester(this, permission)
	permissionRequester.onPermissionChecked = {
		updateViewVisibility(
			ContextCompat.checkSelfPermission(
				requireContext(), permission
			) == PackageManager.PERMISSION_GRANTED
		)
	}
}
```

PermissionRequester 는 fragment 와 필요한 permission 을 인자로 받는다.

구글의 Request runtime permissions 문서의 [Workflow for requesting permissions](https://developer.android.com/training/permissions/requesting#workflow_for_requesting_permissions) 부분을 보면 아래와 같은 workflow 를 확인할 수 있었고 주어진 로직대로 구현했다.

![](https://developer.android.com/static/images/training/permissions/workflow-runtime.svg align="left")

유저가 권한이 필요한 시점까지 권한을 요청하지 않는게 권장되는 방식이기 때문에 권한이 없는채로 Image Picker를 열면 권한을 요청하는 버튼이 나오는 레이아웃이 보이게 했다.

첫 요청에 시스템 권한 요청 다이얼로그가 뜨고 거절하면 왜 해당 권한이 필요한지 설명한다. 또 거절한다면 추가 액션을 취하지 않고 다음 요청 시, 시스템 설정에서 권한을 변경하도록 화면을 이동시키는 로직을 구현했다.

```kotlin
class PermissionRequester(
	private val fragment: Fragment,
	private val permission: String
) {
	
	private lateinit var permissionLauncher: ActivityResultLauncher<String>
	var onPermissionChecked: (() -> Unit)? = null
	
	private val rationale: String
		get() = when (permission) {
			Manifest.permission.READ_EXTERNAL_STORAGE -> "Access to storage is necessary to load and save images."
			Manifest.permission.READ_MEDIA_IMAGES -> "Access to images is necessary to select and save images."
			else -> "Permission is required to use this feature."
		}
	
	init {
		setupPermissionLauncher()
	}
	
	private fun setupPermissionLauncher() {
		permissionLauncher = fragment.registerForActivityResult(ActivityResultContracts.RequestPermission()) { isGranted ->
			handlePermissionResult(isGranted) 
		}
	}
	
	fun requestPermission() {
			permissionLauncher.launch(permission)
	}
	
	private fun handlePermissionResult(isGranted: Boolean) {
		if (isGranted) {
			showSnackbar("Permission Granted")
		} else {
			if (fragment.shouldShowRequestPermissionRationale(permission)) {
				showRationale()
			} else {
				showSettingsRedirect()
			}
		}
		onPermissionChecked?.invoke()
	}
	
	private fun showRationale() {
		Snackbar.make(fragment.requireView(), rationale, Snackbar.LENGTH_INDEFINITE)
			.setAction("OK") {
				permissionLauncher.launch(permission)
			}.show()
	}
	
	private fun showSettingsRedirect() {
		Snackbar.make(fragment.requireView(), "Permission Denied. You can enable it from settings.", Snackbar.LENGTH_LONG)
			.setAction("Settings") {
				val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS)
				val uri = Uri.fromParts("package", fragment.requireContext().packageName, null)
				intent.data = uri
				fragment.startActivity(intent)
			}.show()
	}
	
	private fun showSnackbar(message: String) {  // Refactored: Method to show Snackbar messages
		Snackbar.make(fragment.requireView(), message, Snackbar.LENGTH_SHORT).show()
	}
}
```

코드를 살펴보면

처음 PermissionRequester 가 생성되면 init 를 통해 ActivityResultLauncher 를 초기화한다. ActivityResultLauncher 초기화는 권한 요청 결과에 따라 결과를 어떻게 처리할 것인지 작성한다.

요청 권한에 따라 각각의 rationale 설명을 가져올 수 있도록 when 문을 통해 알맞는 설명을 가져오도록 했다.

### 자 그럼 이제 데이터를 관리하는 ViewModel 을 구현하자

기본적으로 뷰 모델은 전체 이미지 목록, 선택된 이미지 목록, 선택된 이미지 개수를 관리하고 Adapter의 onClick 콜백 로직을 갖고 있다.

편의를 위해 ViewModel 파일 내에 필요한 코드들도 모두 포함시켰다.

첫 코드 스니펫은 internal ImagePickerEvent sealed class 이다.

```kotlin
internal sealed class ImagePickerEvent {

	data class ToggleImage(
		val image: Image
	) : ImagePickerEvent()
	
	data class LongClickImage(
		val image: Image
	) : ImagePickerEvent()
	
	object ReachedMaxSelection : ImagePickerEvent()
	object PermissionGranted : ImagePickerEvent()
}
```

초기 MVVM 으로 구현했었는데 비동기적 상태 변화를 근거로 이벤트의 순차적용을 보장하기 어려워 단방향성 데이터 흐름(UDF)을 가진 MVI(Event)형태로 구현했다.

라이브러리를 사용하는 곳에선 해당 이벤트를 관리해야 할 필요없다고 생각해서 라이브러리 모듈 내에서만 볼 수 있게 internal 로 가시성을 설정했다.

Adapter 에서 보내는 토글 이벤트와 최대 선택 개수 도달, 권한 승인 등의 이벤트를 볼 수 있다.

두 번째 코드 스니펫은 SelectSaveImagePickerViewModel class 이다.

```kotlin
internal class SelectSaveImagePickerViewModel(
	private val maxSelection: Int = 5,
	private val repository: ImageRepository,
) : ViewModel() {
	
	private val _images = MutableStateFlow<List<Image>>(emptyList())
	val images: StateFlow<List<Image>> = _images
	
	private val _event = MutableSharedFlow<ImagePickerEvent>(
		extraBufferCapacity = 10, onBufferOverflow = BufferOverflow.DROP_LATEST
	)
	val event: MutableSharedFlow<ImagePickerEvent> = _event
	
	private var _selectedImages = mutableMapOf<String, Int>()
	val selectedImage: List<String>
		get() = _selectedImages.map { it.key }
	
	private val _selectedImagesCount = MutableStateFlow(0)
	val selectedImagesCount: StateFlow<Int> = _selectedImagesCount
	
	fun handleEvent(event: ImagePickerEvent) {
		when (event) {
			is ImagePickerEvent.LongClickImage -> Unit
			is ImagePickerEvent.ToggleImage -> toggleImageSelection(event.image)
			ImagePickerEvent.PermissionGranted -> permissionGranted()
			else -> Unit
		}
	}
	
	private fun permissionGranted() {
		if(_images.value.isNotEmpty()) return
		loadImages()
	}
	
	fun loadImages() {
		viewModelScope.launch {
			repository.loadImages().collect { newImages ->
				_images.value = newImages
			}
		}
	}
	
	private fun toggleImageSelection(selectedImage: Image) {
		if (_selectedImagesCount.value >= maxSelection && selectedImage.isSelected.not()) {
			_event.tryEmit(ImagePickerEvent.ReachedMaxSelection)
			return
		}
		val currentImages = _images.value.toMutableList()
		val index = currentImages.indexOfFirst { it.uri == selectedImage.uri }
		if (index != -1) {
			val toggledImage = currentImages[index].copy(
				isSelected = !currentImages[index].isSelected,
				selectionOrder = if (currentImages[index].isSelected) -1 else _selectedImages.size + 1
			)
			
			if (toggledImage.isSelected) {
				_selectedImages[toggledImage.uri] = toggledImage.selectionOrder
			} else {
				_selectedImages.remove(toggledImage.uri)
			}
			
			currentImages[index] = toggledImage
			reorderSelections(currentImages)
			updateSelectedImageCount()
		}
	}
	
	private fun updateSelectedImageCount() {
		_selectedImagesCount.value = _selectedImages.size
	}
	
	private fun reorderSelections(updatedList: MutableList<Image>) {
		var currentSelectionOrder = 1
		val selectedImages = updatedList.filter { it.isSelected }.sortedBy { it.selectionOrder }
		
		selectedImages.forEach { selectedImage ->
			val index = updatedList.indexOfFirst { it.uri == selectedImage.uri }
			if (index != -1) {
				updatedList[index] = selectedImage.copy(selectionOrder = currentSelectionOrder++)
			}
		}
		_images.value = updatedList
	}
	
	fun clearSelectedImages() {
		_selectedImages.clear()
		_selectedImagesCount.value = 0
		loadImages()
	}
}
```

위 코드를 살펴보면...

이미지 로드

* Image Picker 를 호출하면 뷰 생성 전 권한 부여 여부에 권한 부여 여부를 근거로 권한 요청이나 이미지 목록 중 하나의 레이아웃을 볼 수 있도록 로직이 구현되어 있다.
    
* 권한 승인이 승인되면 앞서 작성한 PermissionRequester 의 콜백으로 가시성을 변경하고 PermissionGranted 이벤트를 발생시켜 결과에 따라 로컬 이미지를 로드한다.
    

이미지 선택 순서 재정렬

* 이미지가 선택되는 순서에 따라 order 를 늘려가는데 그 크기는 \_selectedImages.size + 1 로 설정한다.
    
* 이미 선택된 이미지를 선택 해제할 때 기존 선택한 이미지 목록을 저장한 리스트를 통해 이전 선택된 이미지의 목록 순서를 재정렬한다.
    

로드된 이미지 제거

* 이미지 목록 제거
    
* 선택된 이미지 제거
    
* 선택된 이미지 개수 제거
    

이미지 선택 개수 제한

* 선택이미지 개수가 최대로 도달한 시점에서 또 다른 true 상태인 item 을 만들려는 상황에선 toggle 되지 않도록 구현.
    
* 이벤트를 발생시켜 최대 개수에 도달한 것을 사용자에게 알림.
    

코드를 보면 왜 선택된 이미지 리스트와 그 개수를 따로 관리하는지 의아할 수 있을 것 같다.

거기엔 나름의 이유가 있는데, 우선 선택된 이미지 상태(selectedImage)는 각각의 이미지 데이터 클래스가 가지므로 선택된 이미지는 이미지 선택/선택해제 시 순서 재정렬외엔 사용하지 않기 때문에 Presentation 에서 수집 필요가 없기 때문에 StateFlow 타입으로 지정하지 않았다. 또 정렬 과정에서 StateFlow 타입을 일반 타입처럼 처리하면서 번거로운 작업을 하고 싶지 않았다.

하지만 선택된 이미지 개수는 화면에 보이기 위해 수집될 필요가 있었고 선택된 이미지 개수는 StateFlow 타입으로 선언하여 Presentation 에서 관찰 할 수 있도록 구현하였다.

현재 1.0.0 버전을 배포한 상태인데 향후 데이터 관리를 map 으로 변경할 계획을 하고 있다.

> 현재는 목록을 가져온 뒤 Add 를 해야지만이 bottom sheet 를 호출하고 찍은 사진을 포함한 새로운 목록을 가져오는데 이후엔 새로운 이미지가 추가되면 목록이 자동으로 업데이트 되는 기능을 추가하고 싶다. 그러기 위해선 업데이트 시 선택된 이미지를 검색 후 상태를 변경하는데까지 시간을 줄이기 위해 데이터를 map으로 관리해야겠다고 생각했다.

세 번째는 ViewModelFactory class 이다. 일반적인 구현이다.

```kotlin
class ViewModelFactory(
	private val maxSelection: Int, private val repository: ImageRepository
) : ViewModelProvider.Factory {
	
	override fun <T : ViewModel> create(modelClass: Class<T>): T {
		if (modelClass.isAssignableFrom(SelectSaveImagePickerViewModel::class.java)) {
			@Suppress("UNCHECKED_CAST") return SelectSaveImagePickerViewModel(
				maxSelection = maxSelection, repository = repository
			) as T
		}
		throw IllegalArgumentException("Unknown ViewModel class")
	}
}
```

또 글이 길어져 Fragment, config 구현과 배포에 대한 내용은 다음 포스팅에 작성하겠다.