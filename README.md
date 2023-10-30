

---
## VisualProgramming
---
MFC의 SDI기반 멀티태스킹 예제 구현 과제
---
- CSDIAppView.h
```ruby
// MySDIAppView.h

protected:
    CDocument* m_pDocuments[3]; // 최대 3개의 문서
    int m_nActiveDoc;

public:
    void CreateNewDocument();
    void SwitchDocument(int nIndex);

protected:
    afx_msg void OnDocument1();
    afx_msg void OnDocument2();
    afx_msg void OnDocument3();
```
- CSDIAppView.cpp
```ruby
// MySDIAppView.cpp

void CMySDIAppView::CreateNewDocument()
{
    int nDocCount = 0;
    for (int i = 0; i < 3; i++)
    {
        if (m_pDocuments[i] != NULL)
            nDocCount++;
    }

    if (nDocCount >= 3)
    {
        AfxMessageBox(_T("최대 3개의 문서만 지원합니다."));
        return;
    }

    CDocument* pDoc = ((CFrameWnd*)AfxGetMainWnd())->CreateNewDocument();
    if (pDoc != NULL)
    {
        m_pDocuments[nDocCount] = pDoc;
        SwitchDocument(nDocCount);
    }
}

void CMySDIAppView::SwitchDocument(int nIndex)
{
    if (nIndex >= 0 && nIndex < 3 && m_pDocuments[nIndex] != NULL)
    {
        CView* pActiveView = GetActiveView();
        CDocument* pActiveDoc = pActiveView->GetDocument();

        if (pActiveDoc != m_pDocuments[nIndex])
        {
            CView* pNewView = m_pDocuments[nIndex]->GetFirstView();
            CFrameWnd* pMainFrame = (CFrameWnd*)AfxGetMainWnd();
            pMainFrame->SetActiveView(pNewView);

            m_nActiveDoc = nIndex;
        }
    }
}

void CMySDIAppView::OnDocument1()
{
    SwitchDocument(0);
}

void CMySDIAppView::OnDocument2()
{
    SwitchDocument(1);
}

void CMySDIAppView::OnDocument3()
{
    SwitchDocument(2);
}
```

-> 문서를 열고 "문서 1", "문서 2", "문서 3" 메뉴를 통해 문서 간 전환을 할 수 있다. 최대 3개의 문서를 멀티태스킹으로 지원하며 사용자가 문서를 전환할 수 있다.
 
---
MFC-FormView 과제
---
<채점기준>
1. 제출 3점 (O)
2. 3개 이상의 컨트롤 사용 +1 (O)
3. 수업에 다루지 않은 컨트롤 활용 +1 (O)
4. 소감이 구체적이거나 감동적일 때 +1 (O)

- My11View.cpp
```ruby

// My11View.cpp: CMy11View 클래스의 구현
//

#include "pch.h"
#include "framework.h"
// SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
// 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
#ifndef SHARED_HANDLERS
#include "My11.h"
#endif

#include "My11Doc.h"
#include "My11View.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// CMy11View

IMPLEMENT_DYNCREATE(CMy11View, CFormView)

BEGIN_MESSAGE_MAP(CMy11View, CFormView)
	// 표준 인쇄 명령입니다.
	ON_COMMAND(ID_FILE_PRINT, &CFormView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_DIRECT, &CFormView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_PREVIEW, &CFormView::OnFilePrintPreview)
	ON_EN_CHANGE(IDC_EDIT1, &CMy11View::OnEnChangeEdit1)
	ON_WM_LBUTTONDOWN()
	ON_WM_MOUSEMOVE()
	ON_COMMAND(ID_SELECT_COLOR, &CMy11View::OnSelectColor)
	ON_NOTIFY(NM_CUSTOMDRAW, IDC_SLIDER1, &CMy11View::OnNMCustomdrawSlider1)
	ON_NOTIFY(UDN_DELTAPOS, IDC_SPIN1, &CMy11View::OnDeltaposSpin1)
	ON_COMMAND(ID_SIZE_1, &CMy11View::OnSize1)
	ON_COMMAND(ID_SIZE_4, &CMy11View::OnSize4)
	ON_COMMAND(ID_SIZE_16, &CMy11View::OnSize16)
	
	ON_BN_CLICKED(IDC_BUTTON1, &CMy11View::OnBnClickedButton1)
END_MESSAGE_MAP()

// CMy11View 생성/소멸

CMy11View::CMy11View() noexcept
	: CFormView(IDD_MY11_FORM)
{
	// TODO: 여기에 생성 코드를 추가합니다.

}

CMy11View::~CMy11View()
{
}
CMFCColorButton m_ColorButton;
void CMy11View::DoDataExchange(CDataExchange* pDX)
{
	CFormView::DoDataExchange(pDX);
	DDX_Control(pDX, IDC_EDIT1, m_Edit);
	DDX_Control(pDX, IDC_SPIN1, m_Spin);
	DDX_Control(pDX, IDC_SLIDER1, m_Slider);
	DDX_Control(pDX, IDC_PROGRESS3, m_Progress);
}

BOOL CMy11View::PreCreateWindow(CREATESTRUCT& cs)
{
	// TODO: CREATESTRUCT cs를 수정하여 여기에서
	//  Window 클래스 또는 스타일을 수정합니다.

	return CFormView::PreCreateWindow(cs);
}

void CMy11View::OnInitialUpdate()
{
	CFormView::OnInitialUpdate();
	GetParentFrame()->RecalcLayout();
	ResizeParentToFit();

}


// CMy11View 인쇄

BOOL CMy11View::OnPreparePrinting(CPrintInfo* pInfo)
{
	// 기본적인 준비
	return DoPreparePrinting(pInfo);
}

void CMy11View::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄하기 전에 추가 초기화 작업을 추가합니다.
}

void CMy11View::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄 후 정리 작업을 추가합니다.
}

void CMy11View::OnPrint(CDC* pDC, CPrintInfo* /*pInfo*/)
{
	// TODO: 여기에 사용자 지정 인쇄 코드를 추가합니다.
}


// CMy11View 진단

#ifdef _DEBUG
void CMy11View::AssertValid() const
{
	CFormView::AssertValid();
}

void CMy11View::Dump(CDumpContext& dc) const
{
	CFormView::Dump(dc);
}

CMy11Doc* CMy11View::GetDocument() const // 디버그되지 않은 버전은 인라인으로 지정됩니다.
{
	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CMy11Doc)));
	return (CMy11Doc*)m_pDocument;
}
#endif //_DEBUG


// CMy11View 메시지 처리기


void CMy11View::OnEnChangeEdit1()
{
	// TODO:  RICHEDIT 컨트롤인 경우, 이 컨트롤은
	// CFormView::OnInitDialog() 함수를 재지정 
	//하고 마스크에 OR 연산하여 설정된 ENM_CHANGE 플래그를 지정하여 CRichEditCtrl().SetEventMask()를 호출하지 않으면
	// 이 알림 메시지를 보내지 않습니다.

	// TODO:  여기에 컨트롤 알림 처리기 코드를 추가합니다.
}

CPoint opnt;
void CMy11View::OnLButtonDown(UINT nFlags, CPoint point){


	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.

	CFormView::OnLButtonDown(nFlags, point);
}

COLORREF col;
int Size;
void CMy11View::OnMouseMove(UINT nFlags, CPoint point)
{
	
	CClientDC dc(this);
	int n = GetDlgItemInt(IDC_EDIT1);
	CPen pen(PS_SOLID, Size, col );
	dc.SelectObject(&pen);
	if (nFlags & MK_LBUTTON) {
		dc.MoveTo(opnt);
		dc.LineTo(point);
	}
		opnt = point;
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.

	CFormView::OnMouseMove(nFlags, point);
}


void CMy11View::OnSelectColor()
{
	CColorDialog dlg;
	if (dlg.DoModal() == IDOK) {
		col = dlg.GetColor();
	}
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CMy11View::OnNMCustomdrawSlider1(NMHDR* pNMHDR, LRESULT* pResult)
{
	LPNMCUSTOMDRAW pNMCD = reinterpret_cast<LPNMCUSTOMDRAW>(pNMHDR);
	*pResult = 0;
	Size = m_Slider.GetPos();

	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	*pResult = 0;
}


void CMy11View::OnDeltaposSpin1(NMHDR* pNMHDR, LRESULT* pResult)
{
	LPNMUPDOWN pNMUpDown = reinterpret_cast<LPNMUPDOWN>(pNMHDR);
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	*pResult = 0;
	Size = m_Spin.GetPos();
}


void CMy11View::OnSize1()
{
	Size = 1;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CMy11View::OnSize4()
{
	Size = 4;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CMy11View::OnSize16()
{
	Size = 16;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}





void CMy11View::OnBnClickedButton1()
{
	m_Progress.SetPos(50);

	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
}
```
실행화면)


![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/8762e1c3-b2f0-4889-af15-8b1d64b13167)

->
1. 단일문서 메뉴로 펜의 색과 사이즈를 설정할 수 있습니다.
2. SpinControl로 펜의 굵기를 설정할 수 있습니다.
3. SliderControl로 펜의 굵기를 설정할 수 있습니다.
4. 수업시간에 배우지 않은 ProgressControl로 "에너지 증가!"버튼을 누르면 게이지가 차오릅니다, 버튼에 Progress를 연결시켜주는 과정이 까다로웠지만, 수업시간에 배웠던 내용을 바탕으로 성공적으로 마무리하였습니다.

---
퀴즈 1-1 펜만들기
---
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/f3e733bf-eece-44cb-8ebb-c2212b63f5aa)

-> 펜의 굵기와 색상의 선택 메뉴
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/8b1b756c-18c0-4d31-b0c1-992e5e5509de)
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/d446b364-9344-49a4-bab9-d2d69d0ee324)

![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/22720296-c6a3-4d8c-92e5-478b2245b1d2)

---
-> 파일 읽기와 저장하기
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/6d5cff1b-8e1f-4d50-89f0-62c78decda7e)

![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/ecda9d60-448e-4b41-b0c4-dc3fba512ef8)

---
-> context메뉴에 펜의 굵기와 색상 선택 메뉴 추가
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/a00c2d61-28b1-4dcf-87aa-829f5439117e)

---
-> 숫자키 1부터 9를 누르면 펜의 굵기가 1부터 9로 바뀜
![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/c466397b-5645-49f0-8cd5-b09fb850f899)

---
- PenDoc.cpp
```ruby

// PenView.cpp: CPenView 클래스의 구현
//

#include "pch.h"
#include "framework.h"
// SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
// 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
#ifndef SHARED_HANDLERS
#include "Pen.h"
#endif

#include "PenDoc.h"
#include "PenView.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// CPenView

IMPLEMENT_DYNCREATE(CPenView, CView)

BEGIN_MESSAGE_MAP(CPenView, CView)
	// 표준 인쇄 명령입니다.
	ON_COMMAND(ID_FILE_PRINT, &CView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_DIRECT, &CView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_PREVIEW, &CView::OnFilePrintPreview)
	ON_WM_MOUSEMOVE()
	ON_COMMAND(ID_SELECT_COLOR, &CPenView::OnSelectColor)
	ON_COMMAND(ID_SIZE_8, &CPenView::OnSize8)
	ON_COMMAND(ID_LINE_4, &CPenView::OnLine4)
	ON_COMMAND(ID_LINE_1, &CPenView::OnLine1)
	ON_COMMAND(ID_SIZE_16, &CPenView::OnSize16)
	ON_COMMAND(ID_SIZE_32, &CPenView::OnSize33)
	ON_WM_CONTEXTMENU()
END_MESSAGE_MAP()

// CPenView 생성/소멸

CPenView::CPenView() noexcept
{
	// TODO: 여기에 생성 코드를 추가합니다.

}

CPenView::~CPenView()
{
}

BOOL CPenView::PreCreateWindow(CREATESTRUCT& cs)
{
	// TODO: CREATESTRUCT cs를 수정하여 여기에서
	//  Window 클래스 또는 스타일을 수정합니다.

	return CView::PreCreateWindow(cs);
}

// CPenView 그리기
#include "CLine.h"
void CPenView::OnDraw(CDC* pDC)
{
	CPenDoc* pDoc = GetDocument();
	ASSERT_VALID(pDoc);
	if (!pDoc)
		return;
	int n = pDoc->m_oa.GetSize();
	for (int i = 0; i < n; i++) {
		CLine* pLine = (CLine*)pDoc->m_oa[i];
		pLine->Draw(pDC);
	}
	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.
}


// CPenView 인쇄

BOOL CPenView::OnPreparePrinting(CPrintInfo* pInfo)
{
	// 기본적인 준비
	return DoPreparePrinting(pInfo);
}

void CPenView::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄하기 전에 추가 초기화 작업을 추가합니다.
}

void CPenView::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄 후 정리 작업을 추가합니다.
}


// CPenView 진단

#ifdef _DEBUG
void CPenView::AssertValid() const
{
	CView::AssertValid();
}

void CPenView::Dump(CDumpContext& dc) const
{
	CView::Dump(dc);
}

CPenDoc* CPenView::GetDocument() const // 디버그되지 않은 버전은 인라인으로 지정됩니다.
{
	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CPenDoc)));
	return (CPenDoc*)m_pDocument;
}
#endif //_DEBUG

COLORREF Col;
int Size;
// CPenView 메시지 처리기
#include "CLine.h"
CPoint opnt;
void CPenView::OnMouseMove(UINT nFlags, CPoint point)
{
	if (nFlags == MK_LBUTTON) {
		CPenDoc* pDoc = GetDocument();
		CLine* pLine = new CLine(opnt, point, Size, Col);
		pDoc->m_oa.Add(pLine);
		Invalidate(FALSE);
	}
	
	opnt = point;
	CView::OnMouseMove(nFlags, point);
	
	
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.

	
}


void CPenView::OnSelectColor()
{
	CColorDialog dlg;
	if (dlg.DoModal() == IDOK) {
		Col = dlg.GetColor();
	}
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize8()
{
	Size = 8;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnLine4()
{
	Size = 4;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnLine1()
{
	Size = 1;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize16()
{
	Size = 16;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize33()
{
	Size = 32;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}



BOOL CPenView::PreTranslateMessage(MSG* pMsg)
{
	if (pMsg->message == WM_KEYDOWN) {
		if (pMsg->wParam == '1') { Size = 1; }
		if (pMsg->wParam == '2') { Size = 2; }
		if (pMsg->wParam == '3') { Size = 3; }
		if (pMsg->wParam == '4') { Size = 4; }
		if (pMsg->wParam == '5') { Size = 5; }
		if (pMsg->wParam == '6') { Size = 6; }
		if (pMsg->wParam == '7') { Size = 7; }
		if (pMsg->wParam == '8') { Size = 8; }
		if (pMsg->wParam == '9') { Size = 9; }
	}
	return CView::PreTranslateMessage(pMsg);
}

typedef int UNIT;
void CPenView::OnContextMenu(CWnd* /*pWnd*/, CPoint point)
{
	CMenu menu;
	menu.CreatePopupMenu();

	// 서브 메뉴 추가: 선 굵기 설정 (기존 메뉴 항목 사용)
	CMenu sizeMenu;
	sizeMenu.CreatePopupMenu();
	
	// ... 기존의 선 굵기 항목들을 추가하세요
	
	menu.AppendMenu(MF_STRING | MF_POPUP, (UNIT)sizeMenu.m_hMenu, _T("Size"));
	sizeMenu.AppendMenu(MF_STRING, ID_LINE_1, _T("Size 1"));
	sizeMenu.AppendMenu(MF_STRING, ID_LINE_4, _T("Size 4"));
	sizeMenu.AppendMenu(MF_STRING, ID_SIZE_8, _T("Size 8"));
	sizeMenu.AppendMenu(MF_STRING, ID_SIZE_16, _T("Size 16"));
	sizeMenu.AppendMenu(MF_STRING, ID_SIZE_32, _T("Size 32"));
	
	// 서브 메뉴 추가: 색상 바꾸기 (기존 메뉴 항목 사용)
	menu.AppendMenu(MF_STRING, ID_SELECT_COLOR, _T("Color"));

	// 메뉴 실행 및 선택한 항목의 명령 ID를 가져옴
	int selectedID = menu.TrackPopupMenu(TPM_LEFTALIGN | TPM_RIGHTBUTTON, point.x, point.y, this);

	// 선택한 항목이 선 굵기인 경우, Size 변수에 저장
	switch (selectedID)
	{
	case ID_LINE_1:
		Size = 1;
		break;
	case ID_LINE_4:
		Size = 4;
		break;
	case ID_SIZE_8:
		Size = 8;
		break;
		// 다른 굵기 옵션들을 추가하세요
	}

	// 화면 좌표로 변환
	ClientToScreen(&point);

	// 메뉴 실행
	menu.TrackPopupMenu(TPM_LEFTALIGN | TPM_RIGHTBUTTON, point.x, point.y, this);

	// TODO: 여기에 메시지 처리기 코드를 추가합니다.
}
```
- PenDoc.cpp
```ruby

// PenDoc.cpp: CPenDoc 클래스의 구현
//

#include "pch.h"
#include "framework.h"
// SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
// 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
#ifndef SHARED_HANDLERS
#include "Pen.h"
#endif

#include "PenDoc.h"

#include <propkey.h>

#ifdef _DEBUG
#define new DEBUG_NEW
#endif

// CPenDoc

IMPLEMENT_DYNCREATE(CPenDoc, CDocument)

BEGIN_MESSAGE_MAP(CPenDoc, CDocument)
END_MESSAGE_MAP()


// CPenDoc 생성/소멸

CPenDoc::CPenDoc() noexcept
{
	// TODO: 여기에 일회성 생성 코드를 추가합니다.

}

CPenDoc::~CPenDoc()
{
}

BOOL CPenDoc::OnNewDocument()
{

	if (!CDocument::OnNewDocument())
		return FALSE;

	// TODO: 여기에 재초기화 코드를 추가합니다.
	// SDI 문서는 이 문서를 다시 사용합니다.
	INT_PTR n = m_oa.GetCount();
	for (INT_PTR i = 0; i < n; i++) {
		delete m_oa[i];
	}
	m_oa.RemoveAll();
	return TRUE;
}




// CPenDoc serialization

void CPenDoc::Serialize(CArchive& ar)
{
	m_oa.Serialize(ar); //CObArray m_oa
}

#ifdef SHARED_HANDLERS

// 축소판 그림을 지원합니다.
void CPenDoc::OnDrawThumbnail(CDC& dc, LPRECT lprcBounds)
{
	// 문서의 데이터를 그리려면 이 코드를 수정하십시오.
	dc.FillSolidRect(lprcBounds, RGB(255, 255, 255));

	CString strText = _T("TODO: implement thumbnail drawing here");
	LOGFONT lf;

	CFont* pDefaultGUIFont = CFont::FromHandle((HFONT) GetStockObject(DEFAULT_GUI_FONT));
	pDefaultGUIFont->GetLogFont(&lf);
	lf.lfHeight = 36;

	CFont fontDraw;
	fontDraw.CreateFontIndirect(&lf);

	CFont* pOldFont = dc.SelectObject(&fontDraw);
	dc.DrawText(strText, lprcBounds, DT_CENTER | DT_WORDBREAK);
	dc.SelectObject(pOldFont);
}

// 검색 처리기를 지원합니다.
void CPenDoc::InitializeSearchContent()
{
	CString strSearchContent;
	// 문서의 데이터에서 검색 콘텐츠를 설정합니다.
	// 콘텐츠 부분은 ";"로 구분되어야 합니다.

	// 예: strSearchContent = _T("point;rectangle;circle;ole object;");
	SetSearchContent(strSearchContent);
}

void CPenDoc::SetSearchContent(const CString& value)
{
	if (value.IsEmpty())
	{
		RemoveChunk(PKEY_Search_Contents.fmtid, PKEY_Search_Contents.pid);
	}
	else
	{
		CMFCFilterChunkValueImpl *pChunk = nullptr;
		ATLTRY(pChunk = new CMFCFilterChunkValueImpl);
		if (pChunk != nullptr)
		{
			pChunk->SetTextValue(PKEY_Search_Contents, value, CHUNK_TEXT);
			SetChunkValue(pChunk);
		}
	}
}

#endif // SHARED_HANDLERS

// CPenDoc 진단

#ifdef _DEBUG
void CPenDoc::AssertValid() const
{
	CDocument::AssertValid();
}

void CPenDoc::Dump(CDumpContext& dc) const
{
	CDocument::Dump(dc);
}
#endif //_DEBUG


// CPenDoc 명령
```
- CLine.cpp
```ruby
#include "pch.h"
#include "CLine.h"

IMPLEMENT_SERIAL(CLine, CObject, 1)


void CLine::Serialize(CArchive& ar)
{
	if (ar.IsStoring())
	{	// storing code
		ar << m_From << m_To <<m_Size << m_Col;
	}
	else
	{	// loading code
		ar >> m_From >> m_To >> m_Size >> m_Col;
	}
}
```
나머지 파일들은 Pen.zip에 넣어두었습니다. 
  




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

    ![image](https://github.com/Jingnxxn/VisualProgramming/assets/96435960/6a4a9a38-991f-458e-810c-3ae152a28f1b)


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

