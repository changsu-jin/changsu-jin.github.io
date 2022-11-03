---
published : true
title:  "RDP(Mac to Windows)환경에서 Mac OS환경과 동일한 단축키 사용하기"
categories:
  - env
tags:
  - karabiner
  - Karabiner-Elements
  - Karabiner-Complex-Modification
  - Remap
last_modified_at: 2021-05-22T08:06:00-05:00
classes: wide
#toc: true
#toc_label: "My Table of Contents"
#toc_icon: "fas fa-list"
---


## 개발은 Mac OS에서 하는데 운영 데이터 조회는 Windows에서 해야 하네...

운영을 하다 보면 DB에 접근해서 데이터 확인을 해야 할 일이 빈번한데 DB에 접근 가능한 환경이 윈도우 기반인 경우가 있습니다.  
개발 작업은 주로 Mac에서 진행을 하고 있고 어쩔 수 없이 RDP를 이용해 원격접속을 이용합니다.  
저 같은 경우 SQL작성 시에 주로 사용하는 프로그램은 [DBeaver](https://dbeaver.io/)입니다.   
[DBeaver](https://dbeaver.io/)는 **Mac OS**버전과 **Windows**버전이 있고 자주 사용하는 단축키는 아래 표와 같이 미묘하게 다릅니다.  
그래서 두 가지 환경을 수시로 전환하면서 쿼리를 짜다보면 <span style="color:orange">생산성이 떨어지고 짜증이 치밀어 오릅니다.  </span>

|Command|Mac OS|Windows|RDP(Mac to Windows)|
|:-----:|:----:|:-----:|:-----------------:|
|Home|⌘ + ← |Fn(Win) + ←|Fn(Mac) + ←|
|End|⌘ + →|Fn(Win) + →|Fn(Mac) + →|
|Page Up|⌘ + ↑|Fn(Win) + ↑|Fn(Mac) + ↑|
|Page Down|⌘ + ↓|Fn(Win) + ↓|Fn(Mac) + ↓|
|Copy|⌘ + C|Ctrl + C|⌘ + C
|Paste|⌘ + V|Ctrl + V|⌘ + V
|Cut|⌘ + X|Ctrl + X|⌘ + X
|Undo|⌘ + Z|Ctrl + Z|⌘ + Z
|한영전환|Right ⌘|한/영|Right ⌥  


> <span style="color:yellow"> **Mac OS** 환경의 단축키와 **RDP(Mac to Windows)** 환경의 단축키를 맞추고 싶다!! </span>


**RDP(Mac to Windows)** 에서 한참동안 작업하다가 단축키가 손에 익은 상태에서 **Mac OS**로 돌아오면 버벅대고 **Mac OS**에서 한 동안 로컬 개발하다가 **RDP(Mac to Windows)** 에서 운영하면 또 버벅대고..
하여간 환경 전환 때마다 계속 버벅이게 됩니다.  

<span style="color:orange">도저히 이렇게는 못해먹겠다 싶어 방법을 찾아보았습니다.</span>  
단축키를 한 쪽 환경에 맞춰야 겠다고 생각했고, 자주 사용하는 **Mac OS**환경에 단축키를 맞추기로 했습니다.

## 먼저 한/영 전환부터

저는 **Mac OS**에서 한영전환을 우측 **Right ⌘**키를 사용하고 있습니다.  
지금은 Mac을 사용하고 있지만 예전에 오랫동안 Windows를 사용 했었습니다.  
스페이스 바로 우측 옆에 키가 한영전환으로 사용되는 거에 너무 익숙합니다.  
그래서 맥을 사용 하면서도 처음부터 그렇게 매핑해두고 사용하고 있습니다.  
딱히 Mac을 사용할 때 우측 **Right ⌘**키를 사용하는 경우가 거의 없기도 하구요.

하지만 **RDP(Mac to Windows)** 에서는 한영전환이 **Right ⌥**키로 매핑됩니다.  
바로 한 칸 차이 지만 실제 몰두 해서 타이핑 할 때 요 한칸 차이 땜에 엄청 버벅이고 있더군요.  
그래서 [**Karabiner**](https://karabiner-elements.pqrs.org/)를 이용해 <span style="color:orange">RDP전용 Profile을 하나 만들고 해당 Profile에선 **Right ⌘**키와 **Right ⌥**키를 서로 스위칭</span> 했습니다.

> Karabiner Profile 추가

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/profile-add.png)


> 한영전환 Key Remapping

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/comman-option-switch.png)


> 한영전환 변경 후 Key Mapping 상태

|Command|Mac OS|Windows|RDP(Mac to Windows)|Karabiner RDP Profile| 
|:-----:|:----:|:-----:|:-----------------:|:-------------------:|
|Home|⌘ + ← |Fn(Win) + ←|Fn(Mac) + ←||
|End|⌘ + →|Fn(Win) + →|Fn(Mac) + →||
|Page Up|⌘ + ↑|Fn(Win) + ↑|Fn(Mac) + ↑||
|Page Down|⌘ + ↓|Fn(Win) + ↓|Fn(Mac) + ↓||
|Copy|⌘ + C|Ctrl + C|⌘ + C||
|Paste|⌘ + V|Ctrl + V|⌘ + V||
|Cut|⌘ + X|Ctrl + X|⌘ + X||
|Undo|⌘ + Z|Ctrl + Z|⌘ + Z||
|한영전환|Right ⌘|한/영|Right ⌥|<span style="color:yellow">Right ⌘</span>|  



## RDP Client tool 옵션의 한계 그리고 Fn(Mac)키와 ⌘키를 스위칭
Mac을 사용하면서 Copy,Paste 등은 항상 **⌘+C,⌘+V**를 사용합니다.  
RDP Client에서도 이 부분을 잘 알고 있어서인지, Client tool에서 옵션으로 제공합니다.

> RDP Client 설정 (참고로 **RDP(Mac to Windows)** 에서 **⌘**키는 **Windows Key**로 인식합니다.)

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/rdp-client.png)


하지만 <span style="color:orange">아쉽게도 **⌘ + 방향키** 조합으로 Home,End,Page Up,Page Down을 사용하는 부분까지는 지원되지 않습니다.</span>  
애초에 <span style="color:orange">목적은 **RDP(Mac to Windows)** 에서도 **Mac OS**와 완전히 동일한 단축키 구성을 맞추는 것</span>입니다.  
그래서 **Fn(Mac)** 키와 **Left ⌘** 키를 스위칭 했습니다.

> Fn(Mac) 과 Left ⌘ 스위칭

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/arrow-key.png)


## karabiner Complex Modification을 이용하여 Copy,Paste 등 자주 사용하는 단축키만 Remapping
<span style="color:orange"> **Fn(Mac)** 키와 **Left ⌘** 키가 스위칭 됨에 따라 **Fn(Mac)+C,Fn(Mac)+V**로 바뀌어 버린 Copy,Paste를 **karabiner Complex Modification**기능을 이용해 **⌘+C,⌘+V** 로 재설정 해주었습니다.</span>  
스위칭을 하지 않고 **⌘+방향키** 를 Home,End,Page Up,Page Down으로 매핑해줄수도 있습니다.  
하지만 **⌘+방향키**조합은 이미 Mac OS에서 사용하는 단축키라서 스위칭 후 Fn키에 매핑하는 방식으로 진행했습니다.   
설정방법은 아래 링크와 캡처화면 및 코드 참고하시어 진행하시면 됩니다.

> karabiner에서 제공하는 문서입니다. [complex modifications example](https://karabiner-elements.pqrs.org/docs/json/typical-complex-modifications-examples) 

> 이미 저장소엔 여러 사람들이 올려 둔 karabiner Complex Modification Rule이 있습니다. [Rule Repostory](https://ke-complex-modifications.pqrs.org) 

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/rules-repository.png)

> Rule은 Json형태로 구성됩니다. [Rule Generator](https://genesy.github.io/karabiner-complex-rules-generator) 

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/rule-generator.png)

> 아래 코드는 제가 작성해서 적용한 cut, copy, paste, undo Rule의 code입니다.

~~~json
{
  "title": "RDP cut, copy, paste, undo",
  "rules": [
    {
      "description": "Fn+x > Cmd+x",
      "manipulators": [
        {
          "from": {
            "key_code": "x",
            "modifiers": {
              "mandatory": [
                "fn"
              ]
            }
          },
          "to": [
            {
              "key_code": "x",
              "modifiers": [
                "command"
              ]
            }
          ],
          "type": "basic"
        }
      ]
    },
    {
      "description": "Fn+c > Cmd+c",
      "manipulators": [
        {
          "from": {
            "key_code": "c",
            "modifiers": {
              "mandatory": [
                "fn"
              ]
            }
          },
          "to": [
            {
              "key_code": "c",
              "modifiers": [
                "command"
              ]
            }
          ],
          "type": "basic"
        }
      ]
    },
    {
      "description": "Fn+v > Cmd+v",
      "manipulators": [
        {
          "from": {
            "key_code": "v",
            "modifiers": {
              "mandatory": [
                "fn"
              ]
            }
          },
          "to": [
            {
              "key_code": "v",
              "modifiers": [
                "command"
              ]
            }
          ],
          "type": "basic"
        }
      ]
    },
    {
      "description": "Fn+z > Cmd+z",
      "manipulators": [
        {
          "type": "basic",
          "from": {
            "modifiers": {
              "mandatory": [
                "fn"
              ]
            },
            "key_code": "z"
          },
          "to": [
            {
              "key_code": "z",
              "modifiers": [
                "command"
              ]
            }
          ]
        }
      ]
    }
  ]
}
~~~

이 코드는 아래 링크로 Generator에 바로 접근할 수도 있습니다.  
[RDP cut, copy, paste, undo](https://genesy.github.io/karabiner-complex-rules-generator/#eyJ0aXRsZSI6IlJEUCBjdXQsIGNvcHksIHBhc3RlLCB1bmRvIiwicnVsZXMiOlt7ImRlc2NyaXB0aW9uIjoiRm4reCA+IENtZCt4IiwibWFuaXB1bGF0b3JzIjpbeyJmcm9tIjp7ImtleV9jb2RlIjoieCIsIm1vZGlmaWVycyI6eyJtYW5kYXRvcnkiOlsiZm4iXX19LCJ0byI6W3sia2V5X2NvZGUiOiJ4IiwibW9kaWZpZXJzIjpbImNvbW1hbmQiXX1dLCJ0eXBlIjoiYmFzaWMifV19LHsiZGVzY3JpcHRpb24iOiJGbitjID4gQ21kK2MiLCJtYW5pcHVsYXRvcnMiOlt7ImZyb20iOnsia2V5X2NvZGUiOiJjIiwibW9kaWZpZXJzIjp7Im1hbmRhdG9yeSI6WyJmbiJdfX0sInRvIjpbeyJrZXlfY29kZSI6ImMiLCJtb2RpZmllcnMiOlsiY29tbWFuZCJdfV0sInR5cGUiOiJiYXNpYyJ9XX0seyJkZXNjcmlwdGlvbiI6IkZuK3YgPiBDbWQrdiIsIm1hbmlwdWxhdG9ycyI6W3siZnJvbSI6eyJrZXlfY29kZSI6InYiLCJtb2RpZmllcnMiOnsibWFuZGF0b3J5IjpbImZuIl19fSwidG8iOlt7ImtleV9jb2RlIjoidiIsIm1vZGlmaWVycyI6WyJjb21tYW5kIl19XSwidHlwZSI6ImJhc2ljIn1dfSx7ImRlc2NyaXB0aW9uIjoiRm4reiA+IENtZCt6IiwibWFuaXB1bGF0b3JzIjpbeyJ0eXBlIjoiYmFzaWMiLCJmcm9tIjp7Im1vZGlmaWVycyI6eyJtYW5kYXRvcnkiOlsiZm4iXX0sImtleV9jb2RlIjoieiJ9LCJ0byI6W3sia2V5X2NvZGUiOiJ6IiwibW9kaWZpZXJzIjpbImNvbW1hbmQiXX1dfV19XX0=)

> 생성한 룰은 Install하여 Karabiner에 적용합니다.

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/rule-add.png)


> 설정 후 **Mac OS** 와 **RDP Profile**모든 단축키 구성이 같아졌습니다.  

|Command|Mac OS|Windows|RDP(Mac to Windows)|Karabiner RDP Profile| 
|:-----:|:----:|:-----:|:-----------------:|:-------------------:|
|Home|⌘ + ←|Fn(Win) + ←|Fn(Mac) + ←|<span style="color:yellow">⌘ + ←</span>|
|End|⌘ + →|Fn(Win) + →|Fn(Mac) + →|<span style="color:yellow">⌘ + →</span>|
|Page Up|⌘ + ↑|Fn(Win) + ↑|Fn(Mac) + ↑|<span style="color:yellow">⌘ + ↑</span>|
|Page Down|⌘ + ↓|Fn(Win) + ↓|Fn(Mac) + ↓|<span style="color:yellow">⌘ + ↓</span>|
|Copy|⌘ + C|Ctrl + C|⌘ + C|<span style="color:yellow">⌘ + C</span>|
|Paste|⌘ + V|Ctrl + V|⌘ + V|<span style="color:yellow">⌘ + V</span>|
|Cut|⌘ + X|Ctrl + X|⌘ + X|<span style="color:yellow">⌘ + X</span>|
|Undo|⌘ + Z|Ctrl + Z|⌘ + Z|<span style="color:yellow">⌘ + Z</span>|
|한영전환|Right ⌘|한/영|Right ⌥|Right ⌘|  


이제 모든 설정이 끝났습니다.  
앞으로는 <span style="color:orange">원격접속시에는 Karabiner Profile을 **RDP Profile**로 변경해서 사용하면 **Mac OS**와 완전히 동일한 단축키 구성</span>으로 사용할 수 있습니다. 

## karabiner Profile 전환의 불편함

그런데 여기까지 진행하고 보니 Karabiner의 Profile 전환이 불편해졌습니다.  
항상 상단바의 아이콘을 클릭하여 Profile을 전환 해야 했습니다.  

> 항상 상단 바로 이동해서 Profile을 변경하는 건 너무 불편하다.

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/profile-change.png)

> 상단 바에 Profile명은 기본으로 노출 되지 않습니다.(옵션을 변경해야 합니다.)

![](https://raw.githubusercontent.com/changsu-jin/image/master/assets/img/show-profile.png)

Profile 전환도 Complex Modification을 이용하여 단축키 지정이 가능합니다.  
<span style="color:orange">Karabiner cli명령어를 이용하여 프로필 전환 단축키</span>를 만들 수 있었습니다.  
아래는 제가 Profile 전환에 사용한 Rule입니다. 

> Switch profile (RDP -> Default)

~~~json
{
  "title": "Change Profile",
  "rules": [
    {
      "description": "Press left-shift+caps_lock to enable default profile",
      "manipulators": [
        {
          "type": "basic",
          "from": {
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            },
            "key_code": "caps_lock"
          },
          "to": [
            {
              "shell_command": "'/Library/Application Support/org.pqrs/Karabiner-Elements/bin/karabiner_cli' --select-profile 'Default'"
            }
          ]
        }
      ]
    }
  ]
}
~~~

[Switch profile (RDP -> Default)](https://genesy.github.io/karabiner-complex-rules-generator/#eyJ0aXRsZSI6IkNoYW5nZSBQcm9maWxlIiwicnVsZXMiOlt7ImRlc2NyaXB0aW9uIjoiUHJlc3MgbGVmdC1zaGlmdCtjYXBzX2xvY2sgdG8gZW5hYmxlIGRlZmF1bHQgcHJvZmlsZSIsIm1hbmlwdWxhdG9ycyI6W3sidHlwZSI6ImJhc2ljIiwiZnJvbSI6eyJtb2RpZmllcnMiOnsibWFuZGF0b3J5IjpbImxlZnRfc2hpZnQiXX0sImtleV9jb2RlIjoiY2Fwc19sb2NrIn0sInRvIjpbeyJzaGVsbF9jb21tYW5kIjoiJy9MaWJyYXJ5L0FwcGxpY2F0aW9uIFN1cHBvcnQvb3JnLnBxcnMvS2FyYWJpbmVyLUVsZW1lbnRzL2Jpbi9rYXJhYmluZXJfY2xpJyAtLXNlbGVjdC1wcm9maWxlICdEZWZhdWx0JyJ9XX1dfV19)

> Switch profile (Default -> RDP)

~~~json
{
  "title": "Change Profile",
  "rules": [
    {
      "description": "Press left-shift+caps_lock to enable RDP profile",
      "manipulators": [
        {
          "type": "basic",
          "from": {
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            },
            "key_code": "caps_lock"
          },
          "to": [
            {
              "shell_command": "'/Library/Application Support/org.pqrs/Karabiner-Elements/bin/karabiner_cli' --select-profile 'RDP'"
            }
          ]
        }
      ]
    }
  ]
}
~~~

[Switch profile (Default -> RDP)](https://genesy.github.io/karabiner-complex-rules-generator/#eyJ0aXRsZSI6IkNoYW5nZSBQcm9maWxlIiwicnVsZXMiOlt7ImRlc2NyaXB0aW9uIjoiUHJlc3MgbGVmdC1zaGlmdCtjYXBzX2xvY2sgdG8gZW5hYmxlIFJEUCBwcm9maWxlIiwibWFuaXB1bGF0b3JzIjpbeyJ0eXBlIjoiYmFzaWMiLCJmcm9tIjp7Im1vZGlmaWVycyI6eyJtYW5kYXRvcnkiOlsibGVmdF9zaGlmdCJdfSwia2V5X2NvZGUiOiJjYXBzX2xvY2sifSwidG8iOlt7InNoZWxsX2NvbW1hbmQiOiInL0xpYnJhcnkvQXBwbGljYXRpb24gU3VwcG9ydC9vcmcucHFycy9LYXJhYmluZXItRWxlbWVudHMvYmluL2thcmFiaW5lcl9jbGknIC0tc2VsZWN0LXByb2ZpbGUgJ1JEUCcifV19XX1dfQ==)


여기까지 해서 **RDP(Mac to Windows)**환경에서 **Mac OS**환경과 완전히 동일하게 단축키를 맞추는 작업을 마쳤습니다.  
저와 비슷한 상황을 겪고 계신 분이 있으시면 도움이 되었으면 좋겠습니다.

