# 3주차 Study [ 2024-11-18 ~ 2024-11-22 ]

**[ 목표 ]**
- 이번주 목표 : MFC 기초 복습
-----

**[ 진행 사항 ]**

윈도우 프로그램은 크게 **초기화** 하는 부분과 **메시지**를 처리하는 부분으로 나눌 수 있다.

실제 프로그램에서 초기화 부분은 WinMain() 함수에서 담당하고 메시지를 처리하는 부분은 WndProc() 함수에서 담당한다.
C/C++에서 프로그램이 main() 함수에서 시작해서 main() 함수가 끝나면 프로그램이 종료되듯이 윈도우 프로그램에서도 WinMain() 함수에서 시작해서 WinMain() 함수가 끝나면 프로그램이 종료된다.

초기화 부분을 담당하는 WinMain() 함수는 먼저 윈도우 클래스를 만들어 등록하고, 그 다음 프레임 윈도우를 생성하여 화면에 표시한다.

- **[ WinMain() 함수의 원형과 초기화 내용 ]**
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPTSTR lpCmdLine, int nCmdShow)
{
윈도우 클래스 생성
윈도우 클래스 등록
프레임 윈도우 생성
프레임 윈도우 화면에 표시
메시지 큐로부터 메시지를 받아 해당 프로시저로 보냄
}

- **[ WinMain() 함수 매개변수 ]**
- WINAPI : 윈도우 애플리케이션
- hInstance : 애플리케이션 프로그램의 ID, 애플리케이션이 구동되면 윈도우 시스템에서 애플리케이션 ID를 부여한다.
- hPrevInstance : 같은 프로그램이 이전에 구동되었을 때 설정되는 인스턴스의 핸들, 값은 항상 NULL, 프로그램의 중복실행을 방지하기 위해 만든 것이지만 윈도우 95이후로부터는 사용하지 않는다.
- lpszCmdLine : 프로그램을 구동할 때 같이 들어오는 매개변수로 실행 파일의 경로 등을 나타내는 문자열 포인터이다.
- nCmdShow : 윈도우가 처음화면에 표시될 때 최대화, 최소화 또는 정상 상태로 보여줄 것인지 결정해주는 매개변수이다.

- **[ WndProc() 함수의 원형과 메시지 처리 형태 ]** // WndProc() 함수는 윈도우 시스템에서 들어온 메시지를 switch 문을 이용하여 처리하는 루틴이다.
LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam)
{
switch(message)
{
해당 메시지에 대한 처리
}
}

LRESULT는 결괏값을 저장하는 32bit 자료형이다.
CALLBACK 함수는 뒤에서 어떤 메시지에 의해 감추어진 형태로 구동되는 함수라는 의미로 역으로 호출받는 함수이다.

WndProc() 함수는 WinMain() 함수에서 직접호출하는 코드는 없다.
WndProc() 함수는 CALLBACK 함수이므로 WinMain() 함수의 while 메시지 루프에 의하여 뒤에서 감추어진 상태로 구동된다.
실제로 WndProc() 함수를 호출하는 함수는 메인 메시지 루프의 DispatchMessage() 함수이다.

- **[ WndProc() 함수 매개변수 ]**
- hwnd : 윈도우의 핸들
- message : WinMain() 함수에서 보내주는 메시지
- wParam, lParam : 메시지와 함께 필요한 정보가 들어오는 매개변수이다.
