소개
당신의 빨간 큐브가 아직 지루합니까? 텍스처를 추가할 차례입니다.

그러나 먼저 텍스처가 무엇이며 실제로 텍스처로 무엇을 할 수 있습니까?

텍스처란 무엇입니까?
텍스쳐는 여러분이 알고 있는 것처럼 기하학의 표면을 덮을 이미지입니다. 많은 유형의 텍스처가 형상의 모양에 다양한 영향을 줄 수 있습니다. 색상에 관한 것만이 아닙니다.

다음은 João Paulo 의 유명한 문 텍스처를 사용하는 가장 일반적인 텍스처 유형입니다 . 그의 작품이 마음에 든다면 그에게 Ko-fi를 사 거나 Patreon 이 되십시오 .

색상 (또는 알베도) 알베도 텍스처는 가장 단순한 것입니다. 텍스처의 픽셀만 가져와 지오메트리에 적용합니다.

/assets/lessons/11/color.jpg

알파 알파 텍스처는 흰색은 보이고 검은색은 보이지 않는 회색조 이미지입니다.

/assets/lessons/11/alpha.jpg

높이 높이 텍스처는 정점을 이동하여 약간의 릴리프를 만드는 회색조 이미지입니다. 보고 싶다면 세분화를 추가해야 합니다.

/assets/lessons/11/height.png

일반 일반 텍스처는 작은 세부 사항을 추가합니다. 정점을 이동하지는 않지만 얼굴이 다른 방향으로 향하고 있다고 생각하도록 빛을 유인합니다. 일반 텍스처는 지오메트리를 세분화할 필요가 없기 때문에 좋은 성능으로 디테일을 추가하는 데 매우 유용합니다.

/assets/lessons/11/normal.jpg

앰비언트 오클루전 앰비언트 오클루전 텍스처는 표면의 틈새에 그림자를 가짜로 만드는 그레이스케일 이미지입니다. 물리적으로 정확하지는 않지만 대비를 만드는 데 확실히 도움이 됩니다.

/assets/lessons/11/ambientOcclusion.jpg

금속성 금속성 텍스처는 금속성(흰색)과 비금속성(검은색) 부분을 지정하는 회색조 이미지입니다. 이 정보는 성찰을 만드는 데 도움이 될 것입니다.

/assets/lessons/11/metalness.jpg

거칠기 거칠기는 금속성과 함께 제공되는 회색조 이미지로 어느 부분이 거칠고(흰색) 어떤 부분이 부드러운지(검은색) 지정합니다. 이 정보는 빛을 분산시키는 데 도움이 될 것입니다. 양탄자는 매우 거칠기 때문에 빛의 반사를 볼 수 없지만 수면의 표면은 매우 매끄럽고 반사되는 빛을 볼 수 있습니다. 여기에서 나무는 투명 코트가 있기 때문에 균일합니다.

/assets/lessons/11/roughness.jpg

PBR
이러한 질감(특히 금속성 및 거칠기)은 우리가 PBR 원칙이라고 부르는 것을 따릅니다. PBR은 물리적 기반 렌더링을 의미합니다. 실제 지침을 따르는 경향이 있는 많은 기술을 재그룹화하여 현실적인 결과를 얻습니다.

다른 많은 기술이 있지만 PBR은 사실적인 렌더링의 표준이 되고 있으며 많은 소프트웨어, 엔진 및 라이브러리에서 이를 사용하고 있습니다.

지금은 텍스처를 로드하는 방법, 텍스처를 사용하는 방법, 적용할 수 있는 변환 및 최적화 방법에 중점을 둘 것입니다. PBR에 대한 자세한 내용은 이후 강의에서 다루겠지만, 궁금한 점이 있으면 여기에서 자세히 알아볼 수 있습니다.

https://marmoset.co/posts/basic-theory-of-physically-based-rendering/
https://marmoset.co/posts/physically-based-rendering-and-you-can-too/
텍스처를 로드하는 방법
이미지의 URL 가져오기
텍스처를 로드하려면 이미지 파일의 URL이 필요합니다.

Webpack을 사용하기 때문에 두 가지 방법으로 얻을 수 있습니다.

이미지 텍스처를 /src/폴더 에 넣고 JavaScript 종속성을 가져오는 것처럼 가져올 수 있습니다.

import imageSource from './image.png'

console.log(imageSource)
자바스크립트
또는 해당 이미지를 /static/폴더 에 넣고 /staticURL에 이미지의 경로( 제외 )를 추가하여 액세스할 수 있습니다 .

const imageSource = '/image.png'

console.log(imageSource)
자바스크립트
주의, 이 /static/폴더는 Webpack 템플릿의 구성 때문에만 작동합니다. 다른 유형의 번들러를 사용하는 경우 프로젝트를 조정해야 할 수 있습니다.

/static/나머지 과정 에서는 폴더 기술을 사용합니다 .

이미지 로드
/static/폴더 에서 방금 본 문 텍스처를 찾을 수 있으며 여러 방법으로 로드할 수 있습니다.

네이티브 자바스크립트 사용
기본 JavaScript를 사용하는 Image경우 먼저 인스턴스 를 만들고 load이벤트를 수신 한 다음 src속성을 변경 하여 이미지 로드를 시작해야 합니다.

const image = new Image()
image.onload = () =>
{
    console.log('image loaded')
}
image.src = '/textures/door/color.jpg'
자바스크립트
'image loaded'콘솔에 가 표시 되어야 합니다 . 보시다시피 경로에 폴더 가 '/textures/door/color.jpg'없는 소스를 설정했습니다 /static.

해당 이미지를 직접 사용할 수 없습니다. 먼저 해당 이미지에서 텍스처 를 생성해야 합니다 .

이는 WebGL이 GPU에서 액세스할 수 있는 매우 구체적인 형식이 필요하고 밉매핑과 같은 텍스처에 일부 변경 사항이 적용되기 때문입니다.

Texture 클래스를 사용하여 텍스처를 만듭니다 .

const image = new Image()
image.addEventListener('load', () =>
{
    const texture = new THREE.Texture(image)
})
image.src = '/textures/door/color.jpg'
자바스크립트
우리가 지금 해야 할 일은 그것을 texture에서 사용하는 것 입니다 material. 불행히도 texture변수는 함수에서 선언되었으며 이 함수 외부에서 변수에 액세스할 수 없습니다. 이것은 범위라는 JavaScript 제한 사항입니다.

함수 내부에 메쉬를 생성할 수도 있지만 함수 외부에 텍스처를 생성한 다음 texture needsUpdate속성을 true다음과 같이 설정하여 이미지가 로드되면 업데이트하는 더 나은 솔루션 이 있습니다 .

const image = new Image()
const texture = new THREE.Texture(image)
image.addEventListener('load', () =>
{
    texture.needsUpdate = true
})
image.src = '/textures/door/color.jpg'
자바스크립트
이렇게 하는 동안 texture변수를 즉시 사용할 수 있으며 이미지는 로드될 때까지 투명합니다.

큐브의 텍스처를 보려면 color속성을 다음으로 바꾸고 값을 map사용합니다 texture.

const material = new THREE.MeshBasicMaterial({ map: texture })
자바스크립트
/assets/lessons/11/step-01.png

큐브의 양쪽에 문 텍스처가 표시되어야 합니다.

텍스처 로더 사용
기본 JavaScript 기술은 그렇게 복잡하지 않지만 TextureLoader를 사용 하면 훨씬 더 간단한 방법이 있습니다 .

TextureLoader 클래스를 사용 하여 변수를 인스턴스화하고 해당 .load(...)메서드를 사용하여 텍스처를 만듭니다.

const textureLoader = new THREE.TextureLoader()
const texture = textureLoader.load('/textures/door/color.jpg')
자바스크립트
내부적으로 Three.js는 이미지를 로드하고 준비가 되면 텍스처를 업데이트하기 위해 이전에 했던 작업을 수행합니다.

하나의 TextureLoader 인스턴스로 원하는 만큼 텍스처를 로드할 수 있습니다 .

경로 뒤에 3개의 함수를 보낼 수 있습니다. 그들은 다음과 같은 행사를 위해 소집될 것입니다:

load 이미지가 성공적으로 로드되었을 때
progress 로딩이 진행 중일 때
error 뭔가 잘못되었다면
const textureLoader = new THREE.TextureLoader()
const texture = textureLoader.load(
    '/textures/door/color.jpg',
    () =>
    {
        console.log('loading finished')
    },
    () =>
    {
        console.log('loading progressing')
    },
    () =>
    {
        console.log('loading error')
    }
)
자바스크립트
텍스처가 작동하지 않으면 이러한 콜백 함수를 추가하여 무슨 일이 일어나고 있는지 확인하고 오류를 찾는 것이 유용할 수 있습니다.

LoadingManager 사용
마지막으로 로드할 이미지가 여러 개 있고 모든 이미지가 로드될 때 알림을 받는 것과 같은 이벤트를 상호화하려는 경우 LoadingManager를 사용할 수 있습니다 .

의 인스턴스 만들기 LoadingManager의 클래스를하고 그것은에 전달 TextureLoader :

const loadingManager = new THREE.LoadingManager()
const textureLoader = new THREE.TextureLoader(loadingManager)
자바스크립트
당신은 당신의 자신의 기능에 의해 다음과 같은 속성을 대체하여 다양한 이벤트를들을 수 있습니다 onStart, onLoad, onProgress, 및 onError:

const loadingManager = new THREE.LoadingManager()
loadingManager.onStart = () =>
{
    console.log('loading started')
}
loadingManager.onLoad = () =>
{
    console.log('loading finished')
}
loadingManager.onProgress = () =>
{
    console.log('loading progressing')
}
loadingManager.onError = () =>
{
    console.log('loading error')
}

const textureLoader = new THREE.TextureLoader(loadingManager)
자바스크립트
이제 필요한 모든 이미지 로드를 시작할 수 있습니다.

// ...

const colorTexture = textureLoader.load('/textures/door/color.jpg')
const alphaTexture = textureLoader.load('/textures/door/alpha.jpg')
const heightTexture = textureLoader.load('/textures/door/height.jpg')
const normalTexture = textureLoader.load('/textures/door/normal.jpg')
const ambientOcclusionTexture = textureLoader.load('/textures/door/ambientOcclusion.jpg')
const metalnessTexture = textureLoader.load('/textures/door/metalness.jpg')
const roughnessTexture = textureLoader.load('/textures/door/roughness.jpg')
자바스크립트
여기에서 볼 수 있습니까? texture변수 이름 colorTexture을 변경했으므로 다음 material에서도 변경하는 것을 잊지 마십시오 .

const material = new THREE.MeshBasicMaterial({ map: colorTexture })
자바스크립트
LoadingManager은 당신이 로더를 보여주고 싶은 모든 자산이로드 된 경우에만 그것을 숨길 경우에 매우 유용합니다. 다음 강의에서 살펴보겠지만, 다른 유형의 로더와 함께 사용할 수도 있습니다.

UV 언래핑
큐브에 텍스처를 배치하는 방법은 매우 논리적이지만 다른 지오메트리에서는 상황이 조금 더 까다로울 수 있습니다.

BoxGeometry를 다른 도형으로 교체해 보십시오.

const geometry = new THREE.BoxGeometry(1, 1, 1)

// Or
const geometry = new THREE.SphereGeometry(1, 32, 32)

// Or
const geometry = new THREE.ConeGeometry(1, 1, 32)

// Or
const geometry = new THREE.TorusGeometry(1, 0.35, 32, 100)
자바스크립트
보시다시피, 텍스처는 지오메트리를 덮기 위해 다양한 방식으로 늘어나거나 압착됩니다.

이를 UV 언래핑 이라고 합니다. 종이접기나 사탕 포장을 풀고 평평하게 만드는 것과 같다고 상상할 수 있습니다. 각 정점은 평평한(일반적으로 정사각형) 평면에서 2D 좌표를 갖습니다.

/assets/lessons/11/uvUnwrapping1.png

geometry.attributes.uv속성 에서 실제로 이러한 UV 2D 좌표를 볼 수 있습니다 .

console.log(geometry.attributes.uv)
자바스크립트
이러한 UV 좌표는 프리미티브를 사용할 때 Three.js에 의해 생성됩니다. 고유한 지오메트리를 만들고 여기에 텍스처를 적용하려면 UV 좌표를 지정해야 합니다.

3D 소프트웨어를 사용하여 형상을 만드는 경우 UV 포장 해제도 수행해야 합니다.

/assets/lessons/11/uvUnwrapping2.png

걱정하지 마세요. 대부분의 3D 소프트웨어에는 트릭을 수행해야 하는 자동 언래핑도 있습니다.

텍스처 변형
하나의 텍스처가 있는 큐브로 돌아가서 해당 텍스처에 어떤 종류의 변형을 적용할 수 있는지 봅시다.

반복하다
당신은 사용하여 질감 반복 할 수 있습니다 repeatA는 부동산, Vector2 가 가지고 의미 x와 y특성을.

다음 속성을 변경해 보십시오.

const colorTexture = textureLoader.load('/textures/door/color.jpg')
colorTexture.repeat.x = 2
colorTexture.repeat.y = 3
자바스크립트
/assets/lessons/11/step-02.png

보시다시피 텍스처가 반복되지 않지만 더 작아지고 마지막 픽셀이 늘어난 것처럼 보입니다.

이는 텍스처가 기본적으로 반복되도록 설정되어 있지 않기 때문입니다. 이를 변경하려면 상수를 사용하여 wrapS및 wrapT속성 을 업데이트해야 합니다 THREE.RepeatWrapping.

wrapSx축을 위한 것입니다
wrapTy축을 위한 것입니다
colorTexture.wrapS = THREE.RepeatWrapping
colorTexture.wrapT = THREE.RepeatWrapping
자바스크립트
/assets/lessons/11/step-03.png

다음을 사용하여 방향을 바꿀 수도 있습니다 THREE.MirroredRepeatWrapping.

colorTexture.wrapS = THREE.MirroredRepeatWrapping
colorTexture.wrapT = THREE.MirroredRepeatWrapping
자바스크립트
/assets/lessons/11/step-04.png

오프셋
당신은 사용하여 질감을 상쇄 할 수 offset도 있습니다 재산 Vector2 로 x및 y속성을. 이것을 변경하면 단순히 UV 좌표가 오프셋됩니다.

colorTexture.offset.x = 0.5
colorTexture.offset.y = 0.5
자바스크립트
/assets/lessons/11/step-05.png

회전
rotation라디안 각도에 해당하는 간단한 숫자인 속성을 사용하여 텍스처를 회전할 수 있습니다 .

colorTexture.rotation = Math.PI * 0.25
자바스크립트
/assets/lessons/11/step-06.png

offset및 repeat속성 을 제거 하면 큐브 면의 왼쪽 하단 모서리 주위에서 회전이 발생하는 것을 볼 수 있습니다.

/assets/lessons/11/step-07.png

즉, 실제로 0, 0UV 좌표입니다. 해당 회전의 피벗을 변경하려면 Vector2center 이기도 한 속성을 사용하여 변경할 수 있습니다 .

colorTexture.rotation = Math.PI * 0.25
colorTexture.center.x = 0.5
colorTexture.center.y = 0.5
자바스크립트
이제 텍스처가 중심에서 회전합니다.

/assets/lessons/11/step-08.png

필터링 및 밉매핑
이 면이 거의 숨겨져 있는 상태에서 큐브의 윗면을 보면 매우 흐릿한 질감을 볼 수 있습니다.

/assets/lessons/11/step-09.png

이는 필터링 및 밉매핑 때문입니다.

밉매핑(또는 공백이 있는 "밉 매핑")은 1x1 텍스처를 얻을 때까지 텍스처의 반 더 작은 버전을 계속해서 생성하는 것으로 구성된 기술입니다. 이러한 모든 텍스처 변형은 GPU로 전송되고 GPU는 가장 적합한 텍스처 버전을 선택합니다.

Three.js와 GPU는 이미 이 모든 것을 처리하고 있으며 사용할 필터 알고리즘을 설정할 수 있습니다. 필터 알고리즘에는 축소 필터와 확대 필터의 두 가지 유형이 있습니다.

축소 필터
축소 필터는 텍스처의 픽셀이 렌더의 픽셀보다 작을 때 발생합니다. 즉, 텍스처가 표면에 비해 너무 커서 덮습니다.

minFilter속성을 사용하여 텍스처의 축소 필터를 변경할 수 있습니다 .

6가지 가능한 값이 있습니다.

THREE.NearestFilter
THREE.LinearFilter
THREE.NearestMipmapNearestFilter
THREE.NearestMipmapLinearFilter
THREE.LinearMipmapNearestFilter
THREE.LinearMipmapLinearFilter
기본값은 THREE.LinearMipmapLinearFilter입니다. 질감이 만족스럽지 않으면 다른 필터를 시도해야 합니다.

우리는 각각을 볼 수는 없지만 THREE.NearestFilter매우 다른 결과를 갖는 를 테스트할 것입니다 .

colorTexture.minFilter = THREE.NearestFilter
자바스크립트
픽셀 비율이 1보다 높은 장치를 사용하는 경우에는 큰 차이를 느끼지 못할 것입니다. 그렇지 않은 경우 이 얼굴이 거의 숨겨진 위치에 카메라를 배치하면 더 자세한 정보와 이상한 인공물을 얻을 수 있습니다.

폴더 checkerboard-1024x1024.png에 있는 텍스처로 테스트 /static/textures/하면 해당 인공물을 더 명확하게 볼 수 있습니다.

const colorTexture = textureLoader.load('/textures/checkerboard-1024x1024.png')
자바스크립트
당신이 보는 인공물을 모아레 패턴 이라고 하며 일반적으로 이를 피하고 싶습니다.

확대 필터
확대 필터는 축소 필터처럼 작동하지만 텍스처의 픽셀이 렌더의 픽셀보다 클 때 작동합니다. 즉, 텍스처가 덮는 표면에 비해 너무 작습니다.

폴더 checkerboard-8x8.png에도 있는 텍스처를 사용하여 결과를 볼 수 있습니다 static/textures/.

const colorTexture = textureLoader.load('/textures/checkerboard-8x8.png')
자바스크립트
/assets/lessons/11/step-12.png

매우 큰 표면에 매우 작은 텍스처이기 때문에 텍스처가 모두 흐릿해집니다.

이것이 끔찍해 보인다고 생각할 수도 있지만 아마도 최선일 것입니다. 효과가 너무 과장되지 않으면 사용자가 눈치채지 못할 수도 있습니다.

magFilter속성을 사용하여 텍스처의 확대 필터를 변경할 수 있습니다 .

가능한 값은 두 가지뿐입니다.

THREE.NearestFilter
THREE.LinearFilter
기본값은 THREE.LinearFilter입니다.

를 테스트하면 THREE.NearestFilter기본 이미지가 유지되고 픽셀화된 텍스처가 표시됩니다.

colorTexture.magFilter = THREE.NearestFilter
자바스크립트
/assets/lessons/11/step-13.png

픽셀화된 텍스처가 있는 Minecraft 스타일을 사용하려는 경우 유리할 수 있습니다.

폴더 minecraft.png에 있는 텍스처를 사용하여 결과를 볼 수 있습니다 static/textures/.

const colorTexture = textureLoader.load('/textures/minecraft.png')
자바스크립트
/assets/lessons/11/step-14.png

이 모든 필터에 대한 마지막 말은 다른 필터 THREE.NearestFilter보다 저렴하며 사용할 때 더 나은 성능을 얻을 수 있다는 것입니다.

minFilter속성에 대한 밉맵만 사용하십시오 . 를 사용하는 경우 THREE.NearestFilter밉맵이 필요하지 않으며 다음을 사용하여 밉맵을 비활성화할 수 있습니다 colorTexture.generateMipmaps = false.

colorTexture.generateMipmaps = false
colorTexture.minFilter = THREE.NearestFilter
자바스크립트
그러면 GPU의 부하가 약간 줄어듭니다.

텍스처 형식 및 최적화
텍스처를 준비할 때 3가지 중요한 요소를 염두에 두어야 합니다.

무게
크기(또는 해상도)
자료
무게
웹사이트를 방문하는 사용자가 해당 텍스처를 다운로드해야 한다는 사실을 잊지 마십시오. .jpg(손실 압축이지만 일반적으로 더 가벼움) 또는 .png(무손실 압축이지만 일반적으로 더 무거움) 웹에서 사용하는 대부분의 이미지 유형을 사용할 수 있습니다 .

허용 가능한 이미지를 얻으려면 일반적인 방법을 적용하되 가능한 한 가볍게 하십시오. TinyPNG (jpg에서도 작동) 또는 모든 소프트웨어와 같은 압축 웹 사이트를 사용할 수 있습니다 .

크기
사용하는 텍스처의 각 픽셀은 이미지의 무게에 관계없이 GPU에 저장되어야 합니다. 그리고 하드 드라이브와 마찬가지로 GPU에도 스토리지 제한이 있습니다. 자동으로 생성된 밉매핑이 저장해야 하는 픽셀의 수를 증가시키기 때문에 더 나쁩니다.

가능한 한 이미지의 크기를 줄이십시오.

우리가 밉매핑에 대해 말한 것을 기억한다면 Three.js는 1x1 텍스처를 얻을 때까지 반복적으로 절반의 작은 버전의 텍스처를 생성합니다. 그 때문에 텍스처 너비와 높이는 2의 거듭제곱이어야 합니다. 이는 Three.js가 텍스처 크기를 2로 나눌 수 있도록 하기 위해 필수입니다.

몇 가지 예 : 512x512, 1024x1024또는512x2048

512, 1024및 2048이 1에 도달 할 때까지 (2)에 의해 분할 될 수있다.

너비나 높이가 2의 거듭제곱 값과 다른 텍스처를 사용하는 경우 Three.js는 가장 가까운 숫자 2의 거듭제곱으로 늘리려고 시도하므로 시각적으로 좋지 않은 결과를 얻을 수 있으며 경고도 표시됩니다. 콘솔에서.

자료
먼저 볼 다른 것들이 있기 때문에 아직 테스트하지 않았지만 텍스처는 투명도를 지원합니다. 아시다시피 jpg 파일에는 알파 채널이 없으므로 png를 사용하는 것이 좋습니다.

또는 향후 학습에서 볼 수 있는 것처럼 알파 맵을 사용할 수 있습니다.

일반 텍스처(보라색 텍스처)를 사용하는 경우 각 픽셀의 빨강, 녹색 및 파랑 채널에 대한 정확한 값을 원할 것입니다. 그렇지 않으면 시각적 결함이 발생할 수 있습니다. 이를 위해서는 무손실 압축이 값을 보존하므로 png를 사용해야 합니다.

텍스처를 찾을 수 있는 곳
불행히도 완벽한 질감을 찾는 것은 항상 어렵습니다. 많은 웹 사이트가 있지만 질감이 항상 올바른 것은 아니며 비용을 지불해야 할 수도 있습니다.

웹에서 검색하여 시작하는 것이 좋습니다. 제가 자주 접속하는 웹사이트를 소개합니다.

poligon.com
3dtextures.me
Arroway-textures.ch
개인적인 용도가 아닌 경우 텍스처를 사용할 수 있는 권한이 있는지 항상 확인하세요.

Photoshop과 같은 사진 및 2D 소프트웨어를 사용하거나 Substance Designer 와 같은 소프트웨어를 사용하여 절차적 텍스처를 사용하여 자신만의 것을 만들 수도 있습니다 .