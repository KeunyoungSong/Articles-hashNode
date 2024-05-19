---
title: "Image Picker Android library 배포하기(1)"
datePublished: Sun May 19 2024 12:55:21 GMT+0000 (Coordinated Universal Time)
cuid: clwdjildq000c08k02lztfeyc
slug: image-picker-android-library-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716123258061/faa0e239-aa8e-4252-876a-1829c76ac6db.png

---

스파르타코딩클럽의 안드로이드 개발 과정의 최종 프로젝트에서 전국 캠핑장 정보와 커뮤니티 기능을 제공하는 프로젝트 CampingMate 를 진행하게 됐다.

그 중엔 주된 커뮤니티 기능을 담당하는 게시판 관련 기능들이 있었고 게시물을 업로드하며 이미지도 함께 올릴 수 있는 기능도 포함되어 있었다.

이미지 업로드를 구현하기 위해 로컬 이미지 경로를 가져올 수 있는 몇몇 라이브러리를 확인해야 했고 github 에 올라와있던 라이브러리들을 찾을 수 있었지만 디자인들이 마음에 들지 않았다.

그러던 중 구글이 Android13 부터 새로 내놓은 Photo Picker를 확인해봤고 아래 기능들을 제공했다.

1. 이미지/비디오를 선택할 수 있다
    
2. 다중 선택 기능
    
3. 최대 선택 개수 제한 기능
    
4. Android 11(API 30 이상) 지원(Backport 제공)
    
5. 권한 요청이 필요 없다
    

역시 구글답게 기능을 커스텀하는 방법도 잘 제공하고 있었고 이미지 선택기에 필요한 많은 기능을 제공했다.

### 다만...

이미지 선택 이후 선택한 이미지 상태를 복원해 이전에 선택한 이미지를 변경하는 기능을 제공하지 않았고 내 경험상 SNS 를 사용할 때 이미지를 잘못 선택하거나 비슷하게 여러장 찍어놓은 이미지를 수정하는 일이 잦았다. 더군다나 BottomSheet 로 Photo Picker를 제공하기 때문에 실수로 시트를 내려버리는 일도 있을 수 있겠다고 생각했고 프로젝트 목표로 정한 최소 SDK 버전을 지원하지도 않기에 일관된 UX/UI 를 지원하기 위해 Custom Image Picker 를 만들게 되었다.

|  | Google's Photo Picker | What I need |
| --- | --- | --- |
| 다중 선택 | O | O |
| 개수 제한 | O | O |
| 최소 버전 지원 | API 30 이상 | API 28 이상 |
| 선택된 이미지 상태 복원 | X | O |

그럼 라이브러리를 만들어보자

# 생각보다 까다로운 UX/UI

일단 안드로이드에서 제공하는 Photo Picker 의 UX/UI는 훌륭하기 때문에 기본 틀로 사용하고 필요한 기능을 추가 구현하는 것으로 계획했다.

바텀시트를 상속받는 클래스를 만들고 로컬 이미지 경로를 가져와 RecyclerView 에 띄워 보여주면 되겠다라고 생각했고 어렵지 않은 과정이라고 생각했다. 다만 구현하면서 깨닫게된 의외의 복병이 있었는데...

1. Google 에서 제공하는 Photo Picker는 이미지를 선택하면 BottomSheet 밑에서 View가 하나 올라온다. -&gt; 이미지 선택 시 올라오는 뷰의 위치를 특정할 수 없음
    
2. BottomSheet 내에 RecyclerView 를 배치하고 있다. -&gt; BottomSheet 의 부모를 constraint layout으로 구현하면 내부의 recycler view 높이가 0dp 로 설정됨
    

별 것 아닌 것 같아보이는 위 두 요구사항을 구현하는데 적지 않은 시간이 걸렸지만 이 과정을 통해 BottomSheet 에 대해 더 알아볼 수 있었다

첫 번째 기능은 내가 `BottomSheetDialogFragment` 를 상속받아 ImagePicker 를 구현하고 있기 때문에 생긴 어려움이었는데 BottomSheet 의 slide 기능은 화면 사이즈의 View 가 사용자에게 보이지 않는 영역 아래에 그려지고 이후 사용자가 시트를 드래그하면서 보이는 영역을 조절하는 방식이다.

### BottomSheet 를 활용해 Image Picker 를 구현해보자

> BottomSheet 를 구현하는 방법에는 두 가지 방법이 있다.  
> 1\. LinearLayout 같은 xml 내 일부 layout을 **BottomSheetBehavior** 를 통해 조작하는 방식  
> 2\. **BottomSheetDialogFragment** 를 확장해 호출하는 방식(내가 사용한 방식)

첫번째 방법을 사용하면 Image Picker 를 사용할 화면에 Image Picker 의 레이아웃을 구현해야했기 때문에 작성한 기능을 라이브러리로 제공할 수 없다고 생각했다. 그래서 클래스를 확장해 호출하는 방식인 두 번째 방법을 선택했다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716108037829/b1431508-6511-4935-8120-4ce10ab0d5f7.png align="center")

Layout Inspector 로 본 `BottomSheetDialogFragment` 를 화면에 띄운 모습이다

> 최상단에 FrameLayout 이 생성되고 내부에 CoordinatorLayout, 그 내부에 touch\_outside, design\_bottom\_sheet 가 자동으로 생성된다.  
> 내가 구현한 layout은 design\_bottom\_sheet 내부에 배치된다

처음엔 무턱대고 이미지가 선택되면 bottomsheet 의 내부 레이아웃에 애니메이션으로 popup 뷰를 보이게 했는데 막상 이미지를 선택하면 **올라온 뷰(popup\_view)는 화면 밑에 가려져 보이지 않았다.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716115627852/db38ce7d-0e33-4a3a-a01c-dc7f5d6a004b.png align="center")

나는 아래와 같이 선택 뷰가 아래서 올라와 엄지로 터치하기 편한 UX/UI 를 원한다

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716117524465/06f8ec6f-5ffd-4d2e-81f3-ec7c6933b22b.png align="center")

### **popup\_view 를 어떻게 항상 보이게 하지?**

이미지 선택 시 팝업 뷰를 유저에게 보이기 위해선 단순히 바텀시트의 바닥 레이아웃에 뷰를 보이게 해서는 안된다는 것을 알았다.

그럼 어떻게 현재 보이는 영역의 바닥에 뷰를 배치할 수 있을까?에 대한 의문이 생긴다.

나는 두 가지 방법을 떠올렸다

1. `BottomSheetDialogFragment` 클래스 확장 시 자동으로 생성되는 상위 계층 레이어를 알고 있다. `viewTreeObserver`를 통해 부모 참조를 타고 Coordinator Layout 에 뷰를 동적으로 배치하고 elevation 수준을 조절하여 bottom sheet 앞에 고정된 뷰를 띄운다.
    
2. BottomSheetBehavior.BottomSheetCallback 의 onStateChanged를 override 하면 BottomSheet 의 상태의 변화를 감지할 수 있다(STATE\_DRAGGING, STATE\_SETTLING, STATE\_EXPANDED, STATE\_COLLAPSED, STATE\_HIDDEN). BottomSheet 의 bottomSheetBehavior 를 가져와 peekHeight 를 고정 값으로 지정한다. **이렇게 하므로써 화면에 보이는 뷰의 크기를 구할 순 없지만 접힌상태(STATE\_COLLAPSED) 에서의 bottom sheet 의 높이를 강제할 수 있게 된다.** 그럼 접힌상태와 펴진상태 각각에 알맞은 y 축 위치에 **popup\_view를 배치시킬 수 있게 된다.**
    
3. Snackbar 는 구현해봤지만 bottom sheet 를 숨기면 바텀 시트가 사라진 이후에 스낵바가 사라져 사용하지 않기로 했다. 어떻게 해도 사라지는 시점을 바텀시트보다 앞당길 수 없었다..
    

### 생각 같이 안되네...

아래는 첫 번째 방법인 부모를 참조하여 Coordinator 에 동적으로 뷰를 배치 후 elevation 을 조절하는 방법을 적용한 것이다. 고정된 위치에 원하는 뷰를 배치할 수 있었지만 뷰 영역 위에 알 수 없는 이상한 영역이 생성된다. (삼성의 안드로이드 1세대 개발자분께 받은 답변으로는 이런 방식의 구현은 전혀 권장되지도 않고 지양해야 한다고 한다. 하지만 구글과 초창기 협업시에는 제공되지 않는 기능을 구현하기 위해 이런 식의 방법이 종종 편법으로 사용되기도 했다고..)

![ㅁㄴㅇㄹㄴㅁㄹ](https://cdn.hashnode.com/res/hashnode/image/upload/v1716117139002/5f067428-597a-49bf-9dc8-3e334e2824d8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716119791300/f82b61a9-0931-438f-987c-5bc4b701a108.png?height=300 align="center")

두 번째 방식으론 원하는 위치에 뷰를 배치하긴 했다. 다만 고정 위치에 배치하는 것이 아닌 각 상태에 맞는 위치로 해당 뷰를 배치하는 것이었기 때문에 좀 어설프지만 애니메이션을 적용해 나름 귀여운대로? 구현할 수 있었다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716120966530/ddfb554d-a80b-4098-a192-0af7f69537ec.gif align="center")

### 구현 성공! 이라고 하기 애매하다. 남은 방법은...

1. ImagePicker를 Fragment 가 아닌 Activity 를 사용한다. 그리고 앞서 언급한 BottomSheet 구현의 첫 번째 방식으로 구현하여 activity 의 bottom 을 참조해 popup\_view 를 배치한다.
    
2. Fragment 로 사용하되 popup\_view 를 하단에 배치하지 않고 BottomSheet 상단에 배치하는 UX/UI 로 변경한다.
    

나는 fragment 로 기능을 제공하고 싶었기 때문에 최종적으로 두 번째 방법을 사용해 구현했다.

### BottomSheet 내에 RecyclerView 를 사용하는 건 왜?

BottomSheet 를 구현하는 레이아웃의 루트 뷰를 Constraint 로 구현하고 내부에 RecyclerView 의 높이와 너비를 0dp 로 배치하면 바텀시트가 올라왔을 때 RecyclerView 의 크기가 0dp으로 잡힌다.

여러 시도 끝에 이 문제를 해결할 2가지 방법을 찾았다

1. Bottom sheet 의 루트 뷰 아래에 RecyclerView 와 함께 높이가 match parent 인 View 를 하나 추가한다. 그럼 bottom sheet 내 크기가 View 에 의해 보전되기 때문에 RecyclerView 의 크기가 잡힌다.
    
2. 루트 뷰를 LinearLayout 을 사용한다. RecyclerView 에는 weight 를 1로 설정한다.
    

나는 두 번째 방법을 활용하였다.

---

내용이 길어져 구체적인 기능 구현과 라이브러리 배포에 대한 내용은 다음 포스팅에 작성하겠다.