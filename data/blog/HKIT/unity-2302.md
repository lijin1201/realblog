---
title: 유니티 개발 (Unity Development)
date: '2023-02-24'
tags: ['unity', 'C#']
draft: false
summary: '이번 주에 유니티에 Minecraft 와 비슷한 지도 생성, 기본 캐릭터 움직임과 DB 연결은 구현했습니다.'
---

# 지도 생성

일정 높낮이 번위에서 무자위하개 지형을 생성했습니다. 여기에 2개 빈도(frequency)의 랜덤 함수를 더하여 를 2D(x,z) 지표 함수를 계산됍니다. 그리고 지면 및 옆면 잔디 표면을 png그림을 표현한다.

![ocean](/static/images/HKIT/Unity-map230217-2.png)

# DB로 실시간위치 전송

처음 Main1 의 Update() 함수에서 캐릭터 실시간 위치(x,z)는 NECIAL 서버에 정송 성공했슴. 그러나 캐릭터 가만히 있을때 도 계속 촤표를 전송하고 있고요. 네트워크 자원 낭비 됐어요. 그래서 CharacterController에서 캐릭터 움직임을 판단하고, 수정 후 움직일 때만 서버에서 전송 구현 성공.

# 저장 UI 화면

게임 중에 키보드로 UI 화면을 호출하고, Save 버튼을 누르면 현재 이름, 위치 정보를 DB에서 저장 완료. 이 기능을 통해 게임 다시 로딩할 때, DB로 현재 상태를 복구할 수 있습니다.
