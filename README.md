# 3주차 Study [ 2024-11-18 ~ 2024-11-22 ]

**[ 목표 ]**
- 이번주 목표 : MFC 기초 복습
-----

**[ 진행 사항 ]**

윈도우 프로그램은 크게 **초기화** 하는 부분과 **메시지**를 처리하는 부분으로 나눌 수 있다.

실제 프로그램에서 초기화 부분은 WinMain() 함수에서 담당하고 메시지를 처리하는 부분은 WndProc() 함수에서 담당한다.
C/C++에서 프로그램이 main() 함수에서 시작해서 main() 함수가 끝나면 프로그램이 종료되듯이 윈도우 프로그램에서도 WinMain() 함수에서 시작해서 WinMain() 함수가 끝나면 프로그램이 종료된다.

초기화 부분을 담당하는 WinMain() 함수는 먼저 윈도우 클래스를 만들어 등록하고, 그 다음 프레임 윈도우를 생성하여 화면에 표시한다.

- **[ WinMain() 함수의 원형과 초기화 내용 ]** <br>
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPTSTR lpCmdLine, int nCmdShow)<br>
{<br>
윈도우 클래스 생성<br>
윈도우 클래스 등록<br>
프레임 윈도우 생성<br>
프레임 윈도우 화면에 표시<br>
메시지 큐로부터 메시지를 받아 해당 프로시저로 보냄<br>
}<br>

- **[ WinMain() 함수 매개변수 ]**
- WINAPI : 윈도우 애플리케이션
- hInstance : 애플리케이션 프로그램의 ID, 애플리케이션이 구동되면 윈도우 시스템에서 애플리케이션 ID를 부여한다.
- hPrevInstance : 같은 프로그램이 이전에 구동되었을 때 설정되는 인스턴스의 핸들, 값은 항상 NULL, 프로그램의 중복실행을 방지하기 위해 만든 것이지만 윈도우 95이후로부터는 사용하지 않는다.
- lpszCmdLine : 프로그램을 구동할 때 같이 들어오는 매개변수로 실행 파일의 경로 등을 나타내는 문자열 포인터이다.
- nCmdShow : 윈도우가 처음화면에 표시될 때 최대화, 최소화 또는 정상 상태로 보여줄 것인지 결정해주는 매개변수이다.

- **[ WndProc() 함수의 원형과 메시지 처리 형태 ]** // WndProc() 함수는 윈도우 시스템에서 들어온 메시지를 switch 문을 이용하여 처리하는 루틴이다.<br>
LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam)<br>
{<br>
switch(message)<br>
{<br>
해당 메시지에 대한 처리<br>
}<br>
}<br>

LRESULT는 결괏값을 저장하는 32bit 자료형이다.
CALLBACK 함수는 뒤에서 어떤 메시지에 의해 감추어진 형태로 구동되는 함수라는 의미로 역으로 호출받는 함수이다.

WndProc() 함수는 WinMain() 함수에서 직접호출하는 코드는 없다.
WndProc() 함수는 CALLBACK 함수이므로 WinMain() 함수의 while 메시지 루프에 의하여 뒤에서 감추어진 상태로 구동된다.
실제로 WndProc() 함수를 호출하는 함수는 메인 메시지 루프의 DispatchMessage() 함수이다.

- **[ WndProc() 함수 매개변수 ]**
- hwnd : 윈도우의 핸들
- message : WinMain() 함수에서 보내주는 메시지
- wParam, lParam : 메시지와 함께 필요한 정보가 들어오는 매개변수이다.

[ 코드 ] : https://github.com/Hancho0/3-study/blob/main/first.cpp
-----

모든 윈도우 애플리케이션의 시작점은 WinMain() 함수이다. WinMain() 함수는 윈도우 클래수 구조체인 WNDCLASSEX 데이터 구조체를 생성하고 구조체 멤버의 값을 채워 초기화한 다음, RegisterClassEx() API 함수를 호출하여 운영체제에 등록한다. 이 데이터 구조체는 윈도우 스타일, 메시지 핸들러의 주소, 인스턴스 핸들, 윈도우 배경 색상, 애플리케이션 아이콘, 그리고 디폴트 커서와 같은 윈도우의 특징을 정의힌다. 즉 윈도우 클래스는 윈도우의 기본 외관 및 특성을 정의 한 것이다.

WndClass 구조체 멈버의 값을 설정할 때 가장 유심히 보아야 할 점은 세 번째 멤버인 윈도우 프로시저를 등록하는 부분이다. WndProc()함수를 아직 구현하지 않았지만, 이 함수를 윈도우 프로시저로 등록하였다.

윈도우 클래스가 등록되면 WinMain() 함수는 CreateWindow() API 함수를 호출하여 애플리케이션의 프레임 윈도우를 생성한다.

CreateWindow()함수는 윈도우 이름, 윈도우 위치, 윈도우 크기에 대한 정보를 넘겨줌으로써 윈도우의 형태를 좀 더 세부적으로 정의한다. 첫 번째 인수는 윈도우 클래스에서 정의한 것과 같은 이름으로 해야 한다.

ShowWindow(hwnd, nCmdShow); // 윈도우를 화면에 보이거나 감추는 기능을 하는 함수
UpdateWindow(hwnd); // 윈도우의 외관을 다시 그려주는 기능을 하는 함수
윈도우를 생성한 후 ShowWindow() 함수와 UpdateWIndow() 함수를 호출하여 왼도우를 화면에 나타나게 한다.

while(GetMessage(&msg,NULL,0,0))
{
TranslateMessage(&msg);
DispatchMessage(&msg);
}

GetMessage() 함수는 메시지 큐에서 메시지를 꺼내와 MSG 데이터 구조체에 저장한다.
TranslateMessage() 함수는 가상 키(virtual-key) 메시지를 문자 메시지로 변환한다.
DispatchMessage() 함수는 내부적으로 윈도우 프로시저 함수를 호출하여 윈도우 프로시저 함수에 메시지를 전달한다.
WM_QUIT 메시지를 받을 때 GetMessage() 함수는 0을 리턴하며, WInMain() 함수가 끝나면서 프로그램이 종료된다.

- **[ 윈도우에서 문자열 표현 방식 ]**
- LPSTR (long pointer string) : char*
- LPCSTR (long pointer constant string) : const char*
- LPWSTR (long pointer wide string) : wchat_t*
- LPCWSTR (long pointer constant wide string) : const wchar_t*
- LPTSTR (long pointer t_string) : TCHAR*
- LPCTSTR (long pointer constant t_string) : const TCHAR*

-**[ MSG 구조체 ]**
MSG 구조체는 메시지 큐에 저장되는 메시지 정보를 담고 있는 구조체이다. WinMain() 함수에서 메시지 루프를 보면, GetMessage() 함수를 볼 수 있다.
이 함수는 메시지 큐에서 메시지를 가져와서 MSG 구조체에 메시지 정보를 입력하게 된다. 그러고 나서 윈도우 프로시저가 이 메시지를 처리하게 된다.

typedef struct tagMSG{<br>
HWND hwnd; //이 메시지를 받아서 처리할 윈도우에 대한 핸들<br>
UINT message; //발생한 메시지를 가지고 있으며, 내부적으로 정수형으로 정의<br>
WPARAM wParam; //메시지에 대한 추가적인 정보를 담고 있으며, 이 내용은 메시지의 종류에 따라 다른 값을 가질 수 있다.<br>
LPARAM lParam; //메시지에 대한 추가적인 정보를 담고 있으며, 이 내용은 메시지의 종류에 따라 다른 값을 가질 수 있다.<br>
DWORD time; //메시지가 발생한 시간을 담고 있다. 우리가 생각하는 시간이 아니라 시스템의 시간이다.<br>
POINT pt; //메시지가 발생했을 때, 화면상의 스크린 좌표를 담고 있다.<br>
} MSG;<br>

-**[ WNDCLASSEX 구조체 ]**
이 구조체에는 윈도우 속성에 대한 정보를 포함한다. 

typedef struct tagWNDCLASSEX{<br>
UINT cbSize; //구조체의 크기를 나타낸다.<br>
UINT style; //윈도우의 스타일을 지정한다. 정숫값의 조합으로 지정된다.<br>
WNDPROC lpfnWndProc; //윈도우 프로시저에 대한 포인터를 지정한다.<br>
int cbClasExtra; //윈도우 클래스의 데이터영역을 나타낸다.<br>
int cbWndExtra; //윈도우의 데이터영역을 나타낸다.<br>
HANDLE hInstance; //프로그램 자체에 대한 즉, 인스턴스에 대한 핸들을 지정한다.<br>
HICON hIcon; //이 윈도우에서 사용될 아이콘에 대한 핸들을 지정한다.<br>
HCURSOR hCursor; //이 윈도우에서 사용할 커서에 대한 핸들을 지정한다.<br>
HBRUSH hbrBackground; //윈도우의 백그라운드 브러시에 대한 핸들을 지정한다.<br>
LPCTSTR lpszMenuName; //윈도우에서 메뉴의 이름을 지정하며, 리소스에서 사용된다.<br>
LPCTSTR lpszClassName; //윈도우 클래스의 이름을 명시한다.<br>
HICON hIConSm; //기본적인 작은 아이콘에 대한 핸들을 지정한다.<br>
} WNDCLASSEX

-**[ PAINTSTRUCT 구조체 ]**
이 구조체는 텍스트나 이미지를 윈도우의 클라이언트 영역에 그리고자 할 때 사용할 정보를 포함한다.

typedef struct tagPAINTSTRUCT {<br>
HDC hdc; // 디스플레이 컨텍스트에 대한 핸들을 지정한다.<br>
BOOL fErase; //윈도우의 백그라운드를 다시 그릴지 지정한다.<br>
RECT rcPaint; //그리고자 하는 영역을 사각형 구조체를 이용해 지정한다.<br>
BOOL fRestore; //시스템에 예약되어 있으며, 내부적으로 사용된다.<br>
BOOL fIncUpdate; //시스템에 예약되어 있으며, 내부적으로 사용된다.<br>
BYTE rgbReserved[16]; //시스템에 예약되어 있으며, 내부적으로 사용된다.<br>
} PAINTSTRUCT;

-**[ RECT 구조체 ]**
이 구조체는 사각형 형태의 좌표를 지정하며, 왼쪽 상단 좌표와 오른쪽 화단 좌표를 저장한다.

typedef struct tagRECT {<br>
LONG left; //사각형 영역의 왼쪽 x 좌표를 명시한다.<br>
LONG top; //사각형 영역의 위쪽 y 좌표를 명시한다.<br>
LONG right; //사각형 영역의 오른쪽 x 좌표를 명시한다.<br>
LONG bottom; //사각형 영역의 아래쪽 y 좌표를 명시한다.<br>
} RECT;

-----
**[ 이번 Study를 통해 느낀점 ]**

MFC 기초 복습 하면서 부족했던 부분을 좀 더 채워보는 기회가 되었던거 같습니다. 책을 변경하면서 기존 책과는 또다른 내용을 배울 수 있는것에 대해 흥미로웠던거 같습니다. 좀더 스터디 해서 응용해서 또다른 프로그램을 만들고 싶단 생각하여 좀더 스터디를 많이 해서 응용프로그램을 짜볼려고 합니다.
