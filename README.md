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



----------------# 간단 코드 설명 ------------------------------



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
  



















----------------# 파일별 코드 설명 ------------------------------





아래는 각 파일의 분석과 주요 내용을 포함한 설명입니다.

---

### **1. `CmcManager.java`**
#### 주요 역할:
- 클라이언트 측에서 IPC를 관리하는 클래스.
- IPC 요청을 처리하고, 서비스와 통신.

#### 주요 메서드:
1. **`requestCmcCalc(CmcCalcRequestParams params)`**:
   - 클라이언트에서 요청한 계산 작업을 처리.
   - `Bundle` 객체에 요청 데이터를 담아 `Message`로 서비스에 전달.

2. **`generateRequestBundle(CmcCalcRequestParams params)`**:
   - IPC 요청 데이터를 번들 형태로 구성.
   - 작업 ID, 연산자, 첫 번째 및 두 번째 숫자를 포함.

3. **`release()`**:
   - 서비스 연결 해제.

4. **`isInstalled(Context context)`**:
   - 서비스 앱이 설치되어 있는지 확인.

---

### **2. `CmcServiceManager.java`**
#### 주요 역할:
- IPC 통신을 실제로 처리하고, 서비스와의 바인딩 및 메시지 송수신.

#### 주요 메서드:
1. **`doConnection(IServiceConnection iServiceConnection)`**:
   - 서비스와의 연결을 초기화.
   - `bindService`를 호출하여 서비스와 연결.

2. **`sendMessage(Message msg)`**:
   - 클라이언트 요청 메시지를 서비스에 전달.
   - 메시지 송신이 실패하면 실패 콜백을 호출.

3. **`responseToApp(int respCode, String respMsg, String result)`**:
   - 서비스로부터 응답 메시지를 수신한 후 클라이언트 콜백으로 전달.

4. **`disConnection()`**:
   - 서비스 연결 해제.

---

### **3. `callback` 패키지**
#### 주요 클래스:
- **`CmcResponseCallback`**:
  - 서비스에서 전달된 응답을 처리하기 위한 콜백 인터페이스.
  - **`onSuccess(Bundle data)`**와 **`onFailure(Bundle error)`** 메서드를 포함.
- **`CmcServiceConnectionCallback`**:
  - 서비스 연결 상태를 처리하기 위한 콜백.
  - 성공 및 실패 시 호출.

---

### **4. `constant` 패키지**
#### 주요 상수 정의:
1. **`CmcBundleKey`**:
   - 번들에서 사용되는 키값들을 정의.
   - 예: `KEY_RESULT_CODE`, `KEY_RESULT_MSG`, `KEY_FIRST_NUMBER`.

2. **`CmcMessageType`**:
   - 메시지 타입을 정의.
   - 예: 요청 타입(`REQ`)과 응답 타입(`RESP`).

3. **`CmcResultCode`**:
   - 결과 코드 상수 정의.
   - 성공(`RESULT_SUCCESS`), 실패(`ERROR_UNKNOWN`) 등.

4. **`CmcResultMsg`**:
   - 결과 메시지를 정의.
   - 예: `SUCCESS`, `ERROR_UNKNOWN`.

---

### **5. `Android_ipc_service_app/app/src/main/java/com/cmc/android/service`**
#### 주요 역할:
- 서비스 측 코드로, IPC 요청을 처리하고 클라이언트에 응답.

#### 주요 파일:
1. **`CmcCalcService.java`**:
   - 서비스가 구현된 클래스.
   - 클라이언트의 요청을 처리하고 결과를 반환.
   - 메시지를 기반으로 수학 계산(덧셈, 뺄셈 등)을 수행.

2. **핵심 처리 흐름**:
   - **클라이언트 요청**:
     - 클라이언트에서 `Message`와 `Bundle`을 통해 요청 데이터를 전달.
   - **서비스 처리**:
     - 전달된 요청 데이터를 읽고, 지정된 연산을 수행.
   - **응답 반환**:
     - 결과를 `Bundle`에 담아 클라이언트로 응답.

---

### **코드의 동작 흐름**
1. **클라이언트**:
   - `CmcManager`가 요청을 번들로 패키징하여 `CmcServiceManager`에 전달.
   - `CmcServiceManager`가 서비스와 바인딩하고 메시지를 전송.

2. **서비스**:
   - 메시지를 수신하고 요청 데이터를 처리.
   - 결과를 번들로 패키징하여 클라이언트로 응답.

3. **응답 처리**:
   - `CmcServiceManager`가 응답 메시지를 처리하여 클라이언트 콜백 호출.

---

### 추가 분석이 필요한 부분
- **서비스의 계산 로직** (`CmcCalcService`).
- **콜백의 활용 방식** (`CmcResponseCallback`, `CmcServiceConnectionCallback`).

필요한 부분에 대해 심화 분석이 필요하면 알려주세요!

