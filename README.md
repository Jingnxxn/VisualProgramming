## VisualProgramming
--------
모덜리스 다이어로그
---
1. 먼저 콘솔앱으로 프로젝트를 생성해 기본 다이어로그 틀을 만들어준다.
   ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/0d51fc2c-23e0-4ed1-a200-7e229fc0ca58)

2. ListBox에 m_List 이름으로 변수추가를 해준다.
   ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/8a2cb74d-019c-49c5-9f8e-88c063beb5b5)

3. 리소스뷰에서 Modeless다이어로그를 삽입해준다.

4. 생성한 Modeless다이어로그에 삽입했던 이름 그대로 클래스 추가를 해준다. 

5. My9에 Modeless헤더파일을 불러오고, Modeless클래스를 만들어주고 추가 기능들을 넣어준다.
   ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/8c105c28-522b-444d-9641-646de3bda485)

6. Modeless 다이어로그 틀을 다음과 같이 만들어준다.
   ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/fbb53a81-58f4-4cf2-9e72-a74af1bcdefd)

7. Edit Control에 변수추가를 다음과 같이 해준다. 컨트롤 -> 값, 이름은 m_Str
   ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/c0084879-9e0b-47eb-9483-e351fb5e7cfb)

8. Add버튼 함수에 입력값이 추가되도록 다음 코드를 추가해준다.
   ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/adfe8614-9098-46bd-8e15-9a992ab951b8)

9. Modeless에서 속성에 들어가 윈도우 클로즈 메시지를 추가해준다.
    ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/ab057245-e26e-46d0-897e-44b938992a25)

10. Modeless에서 클래스 추가 마법사(Ctrl+Shift+x)에 들어가서 PostNcDestroy가상함수를 추가해준다.
    ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/6b7a6157-2a4c-4bf1-bb74-852a0dc7c2f6)

11. 다음과 같은 실행화면을 얻을 수 있다.


------
펜만들기
```ruby

// Pen_2Dlg.cpp: 구현 파일
//

#include "pch.h"
#include "framework.h"
#include "Pen_2.h"
#include "Pen_2Dlg.h"
#include "afxdialogex.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// 응용 프로그램 정보에 사용되는 CAboutDlg 대화 상자입니다.

class CAboutDlg : public CDialogEx
{
public:
	CAboutDlg();

// 대화 상자 데이터입니다.
#ifdef AFX_DESIGN_TIME
	enum { IDD = IDD_ABOUTBOX };
#endif

	protected:
	virtual void DoDataExchange(CDataExchange* pDX);    // DDX/DDV 지원입니다.

// 구현입니다.
protected:
	DECLARE_MESSAGE_MAP()
};

CAboutDlg::CAboutDlg() : CDialogEx(IDD_ABOUTBOX)
{
}

void CAboutDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialogEx::DoDataExchange(pDX);
}

BEGIN_MESSAGE_MAP(CAboutDlg, CDialogEx)
END_MESSAGE_MAP()


// CPen2Dlg 대화 상자



CPen2Dlg::CPen2Dlg(CWnd* pParent /*=nullptr*/)
	: CDialogEx(IDD_PEN_2_DIALOG, pParent)
{
	m_hIcon = AfxGetApp()->LoadIcon(IDR_MAINFRAME);
}

void CPen2Dlg::DoDataExchange(CDataExchange* pDX)
{
	CDialogEx::DoDataExchange(pDX);
	DDX_Control(pDX, IDC_SLIDER1, m_slider);
}

BEGIN_MESSAGE_MAP(CPen2Dlg, CDialogEx)
	ON_WM_SYSCOMMAND()
	ON_WM_PAINT()
	ON_WM_QUERYDRAGICON()
	ON_NOTIFY(NM_CUSTOMDRAW, IDC_SLIDER1, &CPen2Dlg::OnNMCustomdrawSlider1)
	ON_BN_CLICKED(IDC_BUTTON1, &CPen2Dlg::OnBnClickedButton1)
	ON_WM_MOUSEMOVE()
END_MESSAGE_MAP()


// CPen2Dlg 메시지 처리기

BOOL CPen2Dlg::OnInitDialog()
{
	CDialogEx::OnInitDialog();

	// 시스템 메뉴에 "정보..." 메뉴 항목을 추가합니다.

	// IDM_ABOUTBOX는 시스템 명령 범위에 있어야 합니다.
	ASSERT((IDM_ABOUTBOX & 0xFFF0) == IDM_ABOUTBOX);
	ASSERT(IDM_ABOUTBOX < 0xF000);

	CMenu* pSysMenu = GetSystemMenu(FALSE);
	if (pSysMenu != nullptr)
	{
		BOOL bNameValid;
		CString strAboutMenu;
		bNameValid = strAboutMenu.LoadString(IDS_ABOUTBOX);
		ASSERT(bNameValid);
		if (!strAboutMenu.IsEmpty())
		{
			pSysMenu->AppendMenu(MF_SEPARATOR);
			pSysMenu->AppendMenu(MF_STRING, IDM_ABOUTBOX, strAboutMenu);
		}
	}

	// 이 대화 상자의 아이콘을 설정합니다.  응용 프로그램의 주 창이 대화 상자가 아닐 경우에는
	//  프레임워크가 이 작업을 자동으로 수행합니다.
	SetIcon(m_hIcon, TRUE);			// 큰 아이콘을 설정합니다.
	SetIcon(m_hIcon, FALSE);		// 작은 아이콘을 설정합니다.

	// TODO: 여기에 추가 초기화 작업을 추가합니다.

	return TRUE;  // 포커스를 컨트롤에 설정하지 않으면 TRUE를 반환합니다.
}

void CPen2Dlg::OnSysCommand(UINT nID, LPARAM lParam)
{
	if ((nID & 0xFFF0) == IDM_ABOUTBOX)
	{
		CAboutDlg dlgAbout;
		dlgAbout.DoModal();
	}
	else
	{
		CDialogEx::OnSysCommand(nID, lParam);
	}
}

// 대화 상자에 최소화 단추를 추가할 경우 아이콘을 그리려면
//  아래 코드가 필요합니다.  문서/뷰 모델을 사용하는 MFC 애플리케이션의 경우에는
//  프레임워크에서 이 작업을 자동으로 수행합니다.

void CPen2Dlg::OnPaint()
{
	if (IsIconic())
	{
		CPaintDC dc(this); // 그리기를 위한 디바이스 컨텍스트입니다.

		SendMessage(WM_ICONERASEBKGND, reinterpret_cast<WPARAM>(dc.GetSafeHdc()), 0);

		// 클라이언트 사각형에서 아이콘을 가운데에 맞춥니다.
		int cxIcon = GetSystemMetrics(SM_CXICON);
		int cyIcon = GetSystemMetrics(SM_CYICON);
		CRect rect;
		GetClientRect(&rect);
		int x = (rect.Width() - cxIcon + 1) / 2;
		int y = (rect.Height() - cyIcon + 1) / 2;

		// 아이콘을 그립니다.
		dc.DrawIcon(x, y, m_hIcon);
	}
	else
	{
		CDialogEx::OnPaint();
	}
}

// 사용자가 최소화된 창을 끄는 동안에 커서가 표시되도록 시스템에서
//  이 함수를 호출합니다.
HCURSOR CPen2Dlg::OnQueryDragIcon()
{
	return static_cast<HCURSOR>(m_hIcon);
}


int n;
void CPen2Dlg::OnNMCustomdrawSlider1(NMHDR* pNMHDR, LRESULT* pResult)
{
	n = m_slider.GetPos();

	LPNMCUSTOMDRAW pNMCD = reinterpret_cast<LPNMCUSTOMDRAW>(pNMHDR);
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	*pResult = 0;
}

COLORREF col;
void CPen2Dlg::OnBnClickedButton1()
{
	CColorDialog dlg;
	if (dlg.DoModal() == IDOK) {
		col = dlg.GetColor();
	}
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
}

CPoint oPnt;
void CPen2Dlg::OnMouseMove(UINT nFlags, CPoint point)
{
	if (nFlags == MK_LBUTTON) {
		CPen pen(PS_SOLID, n , col);
		CClientDC dc(this);
		dc.SelectObject(&pen);
		dc.MoveTo(oPnt);
		dc.LineTo(point);
	}
	oPnt = point;  // 마우스 포인터의 업데이트
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.

	CDialogEx::OnMouseMove(nFlags, point);
}
```
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/87466dbc-ba32-445e-bf48-80d59d21df49)

