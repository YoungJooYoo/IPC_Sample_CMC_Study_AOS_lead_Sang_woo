## CMC 16기 스터디 자료
직접 안드로이드 라이브러리를 만들어보고 적용하기
### 기능
- Messenger, BoundService를 통한 IPC통신
- 모듈이 적용된 앱에서 서비스가 구현된 앱과 연결
- 더하기, 빼기를 할 수 있는 기능
### 브랜치
- **master**
  - 완성된 코드
- **practice**
  - 기본코드만 있는 브랜치
### 프로젝트 버전
- Gradle : 8.9
- Gradle Plugin : 8.7.2
- Android Studio : Ladybug
- targetSdk : 35



----------------# 코드 설명 ------------------------------



### 주요 파일:
1. **`Android_ipc_lib/cmc_ipc/src/main/java/com/ryusw/ipc/api/CmcManager.java`**
2. **`Android_ipc_lib/cmc_ipc/src/main/java/com/ryusw/ipc/api/CmcServiceManager.java`**
3. **`Android_ipc_lib/cmc_ipc/src/main/java/com/ryusw/ipc/callback/`**
4. **`Android_ipc_lib/cmc_ipc/src/main/java/com/ryusw/ipc/constant/`**
5. **`Android_ipc_service_app/app/src/main/java/com/cmc/android/service/`**



---

### **1. `CmcManager.java`**

#### 역할
- `CmcManager`는 IPC를 위한 주요 인터페이스로서, 앱과 서비스 간 메시지를 처리하고 통신을 관리합니다.

#### 주요 기능
1. **서비스 매니저 초기화**:
   - `CmcServiceManager` 객체를 초기화하여 서비스와의 연결을 처리합니다.

2. **계산 요청 전송**:
   - `requestCmcCalc` 메서드를 통해 IPC로 요청을 보냅니다.
   - 요청 데이터를 번들(Bundle)로 구성하고 메시지(Message) 객체에 설정하여 `CmcServiceManager`에 전달합니다.

3. **에러 번들 생성**:
   - 서비스 연결 실패 시 에러 메시지를 생성하여 반환합니다.

4. **앱 설치 여부 확인**:
   - 특정 애플리케이션 ID를 기반으로 앱이 설치되어 있는지 확인하는 유틸리티 메서드입니다.

#### 코드 구조
- **생성자**:
  - `CmcManager(Context context)`: Context를 기반으로 `CmcServiceManager`를 초기화합니다.
- **주요 메서드**:
  - `requestCmcCalc(CmcCalcRequestParams params)`: IPC 요청을 보냅니다.
  - `generateRequestBundle(CmcCalcRequestParams params)`: IPC 요청에 필요한 데이터를 번들로 구성합니다.
  - `isInstalled(Context context)`: 서비스가 설치되어 있는지 확인합니다.

---

### **2. `CmcServiceManager.java`**

#### 역할
- IPC의 실제 구현을 담당하며, 서비스와 바인딩 및 메시지 전송을 관리합니다.

#### 주요 기능
1. **서비스 연결 관리**:
   - 서비스와의 연결 상태를 확인하고 연결/해제 작업을 처리합니다.

2. **메시지 송수신**:
   - 클라이언트에서 받은 메시지를 서비스로 전달하고, 서비스 응답을 처리합니다.

3. **응답 처리**:
   - `responseToApp` 메서드로 IPC 응답 데이터를 애플리케이션에 전달합니다.

4. **서비스 바인딩**:
   - `bindService`를 사용하여 서비스와 바인딩합니다.

#### 코드 구조
- **생성자**:
  - `CmcServiceManager(Context mContext)`: Context를 기반으로 초기화합니다.
- **주요 메서드**:
  - `doConnection(IServiceConnection iServiceConnection)`: 서비스 연결을 초기화합니다.
  - `sendMessage(Message msg)`: IPC 메시지를 서비스로 전송합니다.
  - `disConnection()`: 서비스 연결을 해제합니다.
  - `responseToApp(int respCode, String respMsg, String result)`: 서비스 응답을 처리합니다.

---

### **코드 흐름**
1. **클라이언트 요청**:
   - `CmcManager`가 요청을 수신하여 번들을 생성하고 메시지를 구성합니다.
   - 메시지는 `CmcServiceManager`로 전달됩니다.

2. **서비스와의 통신**:
   - `CmcServiceManager`는 서비스와 바인딩을 수행하고, 메시지를 IPC를 통해 서비스로 보냅니다.

3. **서비스 응답 처리**:
   - 서비스에서 응답 메시지를 수신하고, 클라이언트 콜백을 호출하여 결과를 전달합니다.

