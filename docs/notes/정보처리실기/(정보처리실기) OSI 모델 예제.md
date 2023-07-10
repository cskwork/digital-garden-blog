
## 이슈 : 
사용자가 특정 웹사이트에 액세스할 수 없다고 보고함

### 물리적 (계층 1):

- 네트워크 장치(예: 스위치, 라우터, 케이블)가 제대로 연결되어 있고 전원이 켜져 있고 제대로 작동하는지 확인. 
- 케이블이 손상되었거나 연결이 느슨하지 않은지 확인.

### 데이터 링크 (계층 2):[​](https://visionary-nasturtium-6a9d9f.netlify.app/docs/uncategorized/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC%20%20OSI%20%EC%B0%B8%EC%A1%B0%20%EB%AA%A8%EB%8D%B8#%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%A7%81%ED%81%AC-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%A0%88%EC%9D%B4%EC%96%B4-2 "Direct link to 데이터 링크 레이어(레이어 2):")

- [[스위치]] 가 MAC 주소를 기반으로 데이터 패킷을 올바르게 전달하고 있는지 확인. 
- 잘못 구성된 VLAN, NIC(네트워크 인터페이스 카드) 문제 또는 기타 데이터 링크 관련 문제가 있는지 확인. 

### 네트워크 (계층 3):[​](https://visionary-nasturtium-6a9d9f.netlify.app/docs/uncategorized/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC%20%20OSI%20%EC%B0%B8%EC%A1%B0%20%EB%AA%A8%EB%8D%B8#%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B3%84%EC%B8%B5%EA%B3%84%EC%B8%B5-3 "Direct link to 네트워크 계층(계층 3):")

- [[라우터(Router)]] IP 주소를 기반으로 데이터 패킷을 올바르게 라우팅하고 있는지 확인. 
- 라우팅 테이블 항목이 잘못되었거나 해당 웹사이트로의 트래픽을 차단할 수 있는 [[액세스 제어 목록(ACL)]]이 있는지 확인. 
- 또한 장치에 IP 주소와 서브넷 마스크가 올바르게 구성되어 있는지 확인. -

### 전송 레이어(계층 4):[​](https://visionary-nasturtium-6a9d9f.netlify.app/docs/uncategorized/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC%20%20OSI%20%EC%B0%B8%EC%A1%B0%20%EB%AA%A8%EB%8D%B8#%EC%A0%84%EC%86%A1-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%A0%88%EC%9D%B4%EC%96%B4-4 "Direct link to 전송 레이어(레이어 4):")

- 웹사이트와의 통신에 영향을 줄 수 있는 TCP 또는 UDP와 같은 전송 프로토콜에 문제가 있는지 확인합니다. 
- 예를 들어, 적절한 포트가 열려 있는지, 방화벽이 필요한 트래픽을 차단하고 있지 않은지 확인.

### 세션 (계층 5):[​](https://visionary-nasturtium-6a9d9f.netlify.app/docs/uncategorized/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC%20%20OSI%20%EC%B0%B8%EC%A1%B0%20%EB%AA%A8%EB%8D%B8#%EC%84%B8%EC%85%98-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%A0%88%EC%9D%B4%EC%96%B4-5 "Direct link to 세션 레이어(레이어 5):")

- 사용자 디바이스와 웹사이트 서버 간에 세션을 설정, 유지 또는 종료하는 데 문제가 있는지 조사합니다. 
- 이 계층이 문제의 원인이 될 가능성은 낮지만 모든 가능성을 염두에 두고 확인하는 것이 좋습니다.

### 프레젠테이션 (계층 6):[​](https://visionary-nasturtium-6a9d9f.netlify.app/docs/uncategorized/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC%20%20OSI%20%EC%B0%B8%EC%A1%B0%20%EB%AA%A8%EB%8D%B8#%ED%94%84%EB%A0%88%EC%A0%A0%ED%85%8C%EC%9D%B4%EC%85%98-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%A0%88%EC%9D%B4%EC%96%B4-6 "Direct link to 프레젠테이션 레이어(레이어 6):")

- 전송되는 데이터가 적절한 형식과 인코딩 또는 암호화되어 있는지 검사.

### 애플리케이션 (계층 7):[​](https://visionary-nasturtium-6a9d9f.netlify.app/docs/uncategorized/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC%20%20OSI%20%EC%B0%B8%EC%A1%B0%20%EB%AA%A8%EB%8D%B8#%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%A0%88%EC%9D%B4%EC%96%B4-7 "Direct link to 애플리케이션 레이어(레이어 7):")

- 마지막으로 문제가 웹 브라우저 또는 프록시 설정과 같이 사용 중인 특정 애플리케이션이나 소프트웨어와 관련이 있는지 확인합니다. 
- 브라우저 캐시를 지우거나 브라우저를 업데이트하거나 다른 브라우저에서 웹사이트에 접속하여 문제가 지속되는지 확인합니다.

OSI 모델의 각 계층을 체계적으로 살펴보고 잠재적인 문제를 확인하면 문제의 근본 원인을 파악하고 적절한 해결책을 구현할 수 있습니다. 
이 예제에서는 레이어 3의 라우터에서 웹사이트에 대한 액세스를 차단하는 잘못된 ACL을 발견했다고 가정해 보겠습니다. ACL을 수정하면 문제를 해결하고 웹사이트에 대한 사용자의 액세스를 복원할 수 있습니다.