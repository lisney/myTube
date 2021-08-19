GreasePencil
==============

Addon
---------
설치 시 TimeScrub 옵션 체크 끈다(단축키 혼선)

![image](https://user-images.githubusercontent.com/30430227/128438995-0aba4100-e048-46a2-bcb2-6d5538b24348.png)

![image](https://user-images.githubusercontent.com/30430227/128439042-0d05f240-8e8f-47f6-b9ad-32ac11863f3b.png)
![image](https://user-images.githubusercontent.com/30430227/128439085-2c7f4e2a-69b5-403a-a22a-dd15307b8a16.png)

![image](https://user-images.githubusercontent.com/30430227/128439128-fb200d56-8c08-4f33-a6cf-cb74233bf55a.png)

> 텍스처 추가

![image](https://user-images.githubusercontent.com/30430227/128440446-b1a2b140-ef9f-419d-890b-f63faffafc9c.png)
![image](https://user-images.githubusercontent.com/30430227/128440669-6a914d2f-f53f-47be-9986-5f29b6fd8bf8.png)

>Box Deform
>


Basic
---------
1.mask

![image](https://user-images.githubusercontent.com/30430227/125914029-d139b2b2-b98a-4c4b-891c-2dd90f1a22f7.png)

![image](https://user-images.githubusercontent.com/30430227/125914155-9c388baf-e7ab-4019-a17d-cbe39ace3b78.png)

* Material Preview 모드 -> solid 모드(흰색 배경)
1. Overlay > Guides>Floor Hide
2. Add > Blank Grease Pencil
3. Object 모드 > Draw 모드>
4. Custom Colors 체크 해제(조명효과 없앰)
5. Layers > 'New Layer' 버튼 클릭 후 생성된 레이어 이름을 'eyeball'로 바꿈
6. Strength(불투명도)를 1로 한 후 원을 그린다(현재 0.6, 나중에 Strength를 수정하려면 SculptMode 에서)
7. Edit 모드에서 두께를 키운다(Alt + s)
8. eyeball Material 과 pupil Material(검정색)을 생성한다
9. eyeball을 복사하고(eyeball Edit모드에서 선택안되게 잠금)
10. 이름을 pupil로 바꾼 후 Edit 모드에서 스케일을 줄여 puipil 머티리얼을 적용한다(Edit모드에서 점들을 선택하고 Assign)
11. pupil에 eyeball Mask를 적용한다음 Edit Mode에서 위치를 옮겨서 확인한다

2. 드로잉

![image](https://user-images.githubusercontent.com/30430227/127612134-5d9e5ae5-ceb2-4a8d-bc10-b4c2a0173505.png)

> Draw Strokes on Back(기존 형태 뒤에 그린다)/ Auto Merge / Add Weight Data / Use Additive Drawing(이전 키프레임에 쌓는다)

## Boundary Stroke (Fill 기능)
> 열린 커브에 색을 채울 때 임시로 닫힌 커브로 그린 후 채운다(Edit 모드에서 제거)
> 단축키 Alt + Draw => Ctrl + Alt + Draw 로 바꾼다

## Tint Tool
> 텍스처 드로잉의 경우 Texture Preview 모드로 바꾸어야 보임
> Mode에서 선택한 대상만 적용됨(Stroke/Fill...)

## Cutter Tool
> Trim 기능

## Edit mode /'v' 분리 / 'm' 다른 레이어로 / Ctrl + 'm' 미러 / Shift + R  반복
![image](https://user-images.githubusercontent.com/30430227/127853367-204caaa1-db74-4402-aab0-0155b9727538.png)

> 그래디언트
![image](https://user-images.githubusercontent.com/30430227/127854060-fcc1546e-11b8-400b-9982-99b2cb5a2db7.png)
![image](https://user-images.githubusercontent.com/30430227/127854407-3b6ed166-db61-463e-92bf-6c907d70ee04.png)
![image](https://user-images.githubusercontent.com/30430227/127854016-c080d82f-82b3-4b2e-8deb-41c033ba337d.png)

## 멀티라인
![image](https://user-images.githubusercontent.com/30430227/127855234-b28945bb-fd59-4554-bd6c-6b3ddf347e34.png)
> 여러 점 선택 후 'e' 익스트루더

## 혜성
![image](https://user-images.githubusercontent.com/30430227/127855867-e85db7fd-234e-46dd-9179-1108f7505f43.png)
![image](https://user-images.githubusercontent.com/30430227/127855836-9fd2a449-b015-426c-93d9-46f89971987e.png)
![image](https://user-images.githubusercontent.com/30430227/127855965-d87e7373-e81f-45b0-ae58-3662f7c14774.png)

## Array Modifier - Randomize
![image](https://user-images.githubusercontent.com/30430227/127859264-99cfb70d-6b88-493d-abdf-1acd4f2d7bc6.png)

## Eye Dropper
> Stroke 머티리얼 생성/ Shift + 클릭(Fill) / Shift + Ctrl(Stroke + Fill)

## Line
> Thickness Profile / 시작 끝 점 굵기 설정
> 단축키 'e' 이어 그리기

## Guide 타입
> 도움선

## Stroke thickness
![image](https://user-images.githubusercontent.com/30430227/127641853-690963b6-70a2-4f9d-9c22-1bbf405ec0c2.png)
> shift + v

## 텡탱볼
![image](https://user-images.githubusercontent.com/30430227/127644242-9e2f87dc-c0da-4edc-8b13-90faa600cf04.png)
![image](https://user-images.githubusercontent.com/30430227/127643978-bdc518ea-eea6-4ffe-83b1-b1a9b617308c.png)
![image](https://user-images.githubusercontent.com/30430227/127644049-27a7d98d-a9ea-469c-8681-6c56aac6a0fc.png)

> 다른 방법

![image](https://user-images.githubusercontent.com/30430227/127646421-d39f95e2-f5f0-461f-91e4-2cbcb3c8978c.png)
![image](https://user-images.githubusercontent.com/30430227/127646734-e5e373e4-5728-4937-a127-431c81b9e0e7.png)
![image](https://user-images.githubusercontent.com/30430227/127646535-fe6a4326-53b1-4932-af64-d01fd3a14b6f.png)

## 얼굴 돌리기
> 레이어로 나누기
> 
![image](https://user-images.githubusercontent.com/30430227/127789476-8302c628-7dd7-4041-978c-6feee3fb17b9.png)
![image](https://user-images.githubusercontent.com/30430227/127789483-a062a9ca-0be1-433a-b4a6-0ca815bc9824.png)

> 키프레임
> 
![image](https://user-images.githubusercontent.com/30430227/127789513-efd74afd-32da-4e58-94ad-93313ef4e946.png)
![image](https://user-images.githubusercontent.com/30430227/127789520-7fc885ef-d278-47b5-b762-cc8db9db6c33.png)

> 프레임 1로 이동 후 Object 모드로 전환, 오토키프레임 끄고 본 생성
> 
![image](https://user-images.githubusercontent.com/30430227/127789549-d598e8fc-87f6-4534-8956-4aecc980072e.png)

> Modifier TimeOffset, PoseMode에서 본을 이동
> 
![image](https://user-images.githubusercontent.com/30430227/127789596-330892db-6f3e-4a65-9811-feb148f241bd.png)
![image](https://user-images.githubusercontent.com/30430227/127789756-65dfd9ea-17c3-477f-9f3e-6f2d0e8fa39c.png)

> Edit모드
> 
![image](https://user-images.githubusercontent.com/30430227/127789823-ea280052-6fb1-4267-a84c-5c191fe203a1.png)
![image](https://user-images.githubusercontent.com/30430227/127790014-d1ca94a0-51a6-4477-9412-c053650ad767.png)

![image](https://user-images.githubusercontent.com/30430227/127790236-23715820-09d0-4a0b-824d-3ffba2614955.png)

## holdout 마스크 머티리얼
![image](https://user-images.githubusercontent.com/30430227/127842987-9ed11e99-0fde-481c-88ea-01d9111db4c7.png)
![image](https://user-images.githubusercontent.com/30430227/127843019-716da9fa-70f0-436e-80aa-323139651f90.png)

## circle 
ctrl + 마우스 휠 => 다각형

## Layer Auto Lock
![image](https://user-images.githubusercontent.com/30430227/127851954-c7d58c33-3a58-4f02-8aa8-9fe7941a76a1.png)

## Maker 'm', 글자수정 Ctrl + 'm'
![image](https://user-images.githubusercontent.com/30430227/127942083-8ba49424-4584-495c-a9e6-79f3db32ff58.png)


## Tint Modifier Fake DOF
![image](https://user-images.githubusercontent.com/30430227/127942672-64c3c04a-afc4-424b-8faa-4a99a43d6b12.png)
![image](https://user-images.githubusercontent.com/30430227/127942695-66607307-2f24-483a-8109-18771bc7ade3.png)

## Offset Modifier
![image](https://user-images.githubusercontent.com/30430227/127942750-506a929c-29c0-4f83-ba51-ccf2fa5d30e0.png)
![image](https://user-images.githubusercontent.com/30430227/127942735-184b3a48-6075-4bf3-b7da-da14c23489ca.png)


## Constraint/ Floor
![image](https://user-images.githubusercontent.com/30430227/127953747-834a10d3-f583-4edb-a117-86fc3a3c8c22.png)
![image](https://user-images.githubusercontent.com/30430227/127953766-e167454c-c877-49ae-9b4d-268be528d371.png)

## Constraint/ Maintain Volume
![image](https://user-images.githubusercontent.com/30430227/127954124-bcd1f76d-54cd-4c7a-af59-c6030d450c72.png)
![image](https://user-images.githubusercontent.com/30430227/127954083-e6c5d6b1-9e8a-49d5-b693-73645963d11c.png)
![image](https://user-images.githubusercontent.com/30430227/127954111-6bc2122f-ecbb-4733-8803-04bb40059691.png)

## Constraint 근경/중경/원경
![image](https://user-images.githubusercontent.com/30430227/127954824-895c29b3-8d5a-4642-83b2-1f072b71c9b6.png)
![image](https://user-images.githubusercontent.com/30430227/127954836-688dab93-de20-4c18-aed8-d22abb735d52.png)
![image](https://user-images.githubusercontent.com/30430227/127954851-a576910d-2c65-4e34-854f-318e3484f481.png)

## Convert to
![image](https://user-images.githubusercontent.com/30430227/127955179-7eae9741-0ef3-4299-84d4-9c6be9ea042b.png)
![image](https://user-images.githubusercontent.com/30430227/127955148-fd3f6869-a037-498d-856c-5e4abe275046.png)

## 자동차 와이프 회전
> #cos(frame) -> cos(frame*pi/20)
![image](https://user-images.githubusercontent.com/30430227/127959774-67560b78-4fc4-4573-8a71-c9375f3e9ed4.png)

## 로토스코핑 Camera Background
![image](https://user-images.githubusercontent.com/30430227/128616610-337c92a5-058e-4b92-9767-03dd986c3530.png)

Tip
-----
1. Convert Curve
기존 Ctrl + C는 안먹힌다 > Object 메뉴
![image](https://user-images.githubusercontent.com/30430227/129989694-bbbb5b20-4f56-491a-a5a4-65568dadb014.png)
새로운 커브가 생성된다 > 심플리파이 기능이 없으므로 Checker Deselect 후 Invert, Delete
![image](https://user-images.githubusercontent.com/30430227/129989804-0235ef7f-f6d6-4a29-ab8a-abf253d13058.png)




