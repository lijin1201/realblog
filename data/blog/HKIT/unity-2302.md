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

# 서버로 실시간위치 전송

처음 Main1 의 Update() 함수에서 캐릭터 실시간 위치(x,z)는 NECIAL 서버에 정송 성공했슴. 그러나 캐릭터 가만히 있을때 도 계속 촤표를 전송하고 있고요. 네트워크 자원 낭비 됐어요. 그래서 CharacterController에서 캐릭터 움직임을 판단하고, 수정 후 움직일 때만 서버에서 전송 구현 성공.

```cs
if (moveDirection.magnitude > 0 )
        {
            mCf.mObjP.mName = pName;
            mCf.mObjP.mX = transform.position.x;

            mCf.mObjP.mZ = transform.position.z;

            mCf.mObjP.posiSend(mCf.mCt);
        }
```

# 저장 UI 화면, 위치 DB로전송

게임 중에 키보드로 UI 화면을 호출하고, Save 버튼을 누르면 현재 이름, 위치 정보를 DB에서 저장 완료. 이 기능을 통해 게임 다시 로딩할 때, DB로 현재 상태를 복구할 수 있습니다.

DB저장코드는 여기:
```cs
  public static UpdateResult DbEx_UpdateObjPos(string objName, float[] pos)
        {
            var db1 = DbCon1();
            var clc1 = db1.GetCollection<BsonDocument>(Clcn);
            var ft1 = Builders<BsonDocument>.Filter.Eq("Name", objName);
            var update = Builders<BsonDocument>.Update.Set("PosX", pos[0]).Set("PosZ", pos[1]);
            var upsert = new UpdateOptions { IsUpsert = true };
            return clc1.UpdateOne(ft1, update, upsert);
        }
```

UI호출코드는 여기:
```cs
    void Update()
    {
		if (Input.GetKeyDown(KeyCode.U))
		{ // if you press the E key
			uiActive = !uiActive; // change the state of your bool
			uiCanvas.SetActive(uiActive); // display or not the canvas (following the state of the bool)
			GameObject.Find("Camera").GetComponent<MouseLook>().enabled = !uiActive;
			if (uiActive)
			{
				Cursor.lockState = CursorLockMode.None;
				Cursor.visible = true;
			} else
            {
				Cursor.lockState = CursorLockMode.Locked;
			}
		}
    //...
    }
```