# TRObject-ile-Kitap-Takip-Sistemi

///////////////////////////////////////////////////////// Main Code /////////////////////////////////////////////////////////////////////////////////////
var
  Form1 :TclForm;
  AnaPanel, GirisPaneli : TclProPanel;
  BtnGiris,BtnTikla, BtnKayitOl,BtnSifreSimgesi :TCLProButton;
  EdtKullaniciAdi, EdtKullaniciSifre : TclProEdit;
  label1,label2: TClProLabel;
testImg,kullaniciImg, logoImg:TCLProImage;
G_KullaniciAdi:String;
void KullaniciGirisiYap(AKullaniciAdi, AKullaniciSifre);
{
  try
    Clomosy.DBSQLiteQuery.Sql.Text = '
      SELECT * from Kullanicilar where kullaniciAdi= '+ QuotedStr(AKullaniciAdi) + 'and kullaniciSifre=' + QuotedStr(AKullaniciSifre);
      Clomosy.DBSQLiteQuery.OpenOrExecute;
    
    if (not Clomosy.DBSQLiteQuery.Eof)
        {
         G_KullaniciAdi=EdtKullaniciAdi.Text;
        Clomosy.RunUnit('uIslemler');
        ShowMessage(' Login successful.');
        
        }
        else
          ShowMessage(' Login failed.');
  except
    ShowMessage('004: Hata Sınıfı: '+LastExceptionClassName+' Hata Mesajı: '+LastExceptionMessage);
  }
}

void KullaniciKaydiYap(AKullaniciAdi, AKullaniciSifre);
{
  try
    Clomosy.DBSQLiteQuery.Sql.Text = '
    INSERT INTO Kullanicilar (kullaniciAdi, kullaniciSifre) VALUES ('+QuotedStr(AKullaniciAdi)+', '+QuotedStr(AKullaniciSifre)+');';
    Clomosy.DBSQLiteQuery.OpenOrExecute;
    ShowMessage('User registered.');
  
  except
    ShowMessage('003: Hata Sınıfı: '+LastExceptionClassName+' Hata Mesajı: '+LastExceptionMessage);
  }
}

void KullaniciKontrol;
 var
   TiklananButon:TClProButton;
{
  TiklananButon = TClProButton(Form1.Clsender); 

  if((EdtKullaniciAdi.Text <>'' ) && (EdtKullaniciSifre.Text <>'' ))
  {
    if(TiklananButon.clTagStr == 'KayitOl')
      KullaniciKaydiYap(EdtKullaniciAdi.Text,EdtKullaniciSifre.Text);
    else if (TiklananButon.clTagStr == 'Giris')
      KullaniciGirisiYap(EdtKullaniciAdi.Text,EdtKullaniciSifre.Text);
  
  EdtKullaniciAdi.Text = '';
  EdtKullaniciSifre.Text = '';
    
  }
  else ShowMessage('Username and password should not be left blank.');
}
  
  void SQLiteBaglantiTablosuOlustur;
  var
    TabloVarMi: Boolean;
  {
    try
      Clomosy.DBSQLiteConnect(Clomosy.AppFilesPath + 'KullanicilarDB.db3', '' );
 
 
      Clomosy.DBSQLiteQuery.Sql.Text = 'SELECT name FROM sqlite_master WHERE type="table" AND name="Kullanicilar";';
      Clomosy.DBSQLiteQuery.OpenOrExecute;
      
    
      TabloVarMi = not Clomosy.DBSQLiteQuery.Eof;
      
      if not (TabloVarMi)
      {
        Clomosy.DBSQLiteQuery.Sql.Text = 'CREATE TABLE Kullanicilar(
        kullaniciID INTEGER PRIMARY KEY AUTOINCREMENT,
        kullaniciAdi TEXT ,
        kullaniciSifre TEXT )';
        Clomosy.DBSQLiteQuery.OpenOrExecute;
        
        ShowMessage('Table successfully added to database!');
        
        try
          Clomosy.DBSQLiteQuery.Sql.Text = '
          INSERT INTO Kullanicilar (kullaniciAdi, kullaniciSifre) VALUES (''gulsum'', ''123'');
          INSERT INTO Kullanicilar (kullaniciAdi, kullaniciSifre) VALUES (''Glsm'', ''1234'');';
          Clomosy.DBSQLiteQuery.OpenOrExecute;
          ShowMessage('Data insertion successful!');
        
        except
          ShowMessage('001: Hata Sınıfı: '+LastExceptionClassName+' Hata Mesajı: '+LastExceptionMessage);
        }
        ShowMessage('Data is added to the existing Users table.');
      }

    except
     ShowMessage('002: Hata Sınıfı: '+LastExceptionClassName+' Hata Mesajı: '+LastExceptionMessage);
    }
  }
  
  
   
 void Tiklandigindasifrebutonudegisme;
{
 BtnTikla = TclProButton(Form1.ClSender);

  if (EdtKullaniciSifre.Password == True)
  {
    Form1.SetImage(BtnTikla, 'https://r.resimlink.com/_twLODGPxgXC.png'); 
    EdtKullaniciSifre.Password = False; 
  }
  else
  {
    Form1.SetImage(BtnTikla, 'https://r.resimlink.com/HY-oB0.png'); 
    EdtKullaniciSifre.Password = True; 
  }
}



{
  Form1 = TclForm.Create(Self);
  Form1.SetFormColor('f0f9ff','',clGNone);
  
  Form1.clsetCaption('Giriş Sayfası');
  SQLiteBaglantiTablosuOlustur;
  
  AnaPanel = Form1.AddNewProPanel(Form1,'AnaPanel');
  AnaPanel.ALign = AlCenter;
  AnaPanel.Height = Form1.clHeight / 3;
  AnaPanel.Width = Form1.clWidth - 20;
  AnaPanel.clProSettings.BackgroundColor = nil;
  AnaPanel.SetclProSettings(AnaPanel.clProSettings);
  
  
  logoImg= Form1.AddNewProImage(Form1,'logoImg');
  logoImg.Height =60; 
  logoImg.Align=AlCenter;
  logoImg.Width = 60;
  
  logoImg.Margins.bottom=580;
  Form1.setImage(logoImg,'https://i.pinimg.com/564x/cf/b1/f0/cfb1f0d10fb06f1199e63c971b759e0b.jpg');
   
  
  EdtKullaniciAdi = Form1.AddNewProEdit(AnaPanel,'EdtKullaniciAdi', 'User Name');
  EdtKullaniciAdi.Align=alMostTop;
  EdtKullaniciAdi.Height = AnaPanel.Height / 4; 
  EdtKullaniciAdi.Margins.Top= 5;
  EdtKullaniciAdi.Margins.Left= 10;
  EdtKullaniciAdi.Margins.Left= 10;
  EdtKullaniciAdi.clProSettings.BorderColor = clAlphaColor.clHexToColor('#030303');
  EdtKullaniciAdi.clProSettings.BorderWidth = 1;
  EdtKullaniciAdi.clProSettings.RoundHeight = 20;
  EdtKullaniciAdi.clProSettings.RoundWidth = 20;
  EdtKullaniciAdi.clProSettings.IsRound = True;
  EdtKullaniciAdi.SetclProSettings(EdtKullaniciAdi.clProSettings);
  
    
  kullaniciImg= Form1.AddNewProImage(Form1,'kullaniciImg');
  kullaniciImg.Height = AnaPanel.Height / 4; 
  
 
  kullaniciImg.Height =25;
  kullaniciImg.Width = 25;
  kullaniciImg.Margins.left=280;
  kullaniciImg.Margins.bottom=170;
 Form1.setImage(kullaniciImg,'https://cdn-icons-png.flaticon.com/512/709/709699.png');
   
   
  
  
  EdtKullaniciSifre = Form1.AddNewProEdit(AnaPanel,'EdtKullaniciSifre', 'Password');
  EdtKullaniciSifre.Align=alMostTop;
  EdtKullaniciSifre.Height = AnaPanel.Height / 4;
  EdtKullaniciSifre.Margins.Top= 5;
  EdtKullaniciSifre.Margins.Left= 10;
  EdtKullaniciSifre.SetclProSettings(EdtKullaniciAdi.clProSettings);
  
label1 = Form1.AddNewProLabel(Form1, 'label1', 'WELCOME TO');
clComponent.SetupComponent(label1, '{"Align":"Center",
"MarginBottom":470, 
"Width":200, 
"Height":30, 
"TextColor":"#333333",
"TextSize":30,
"TextVerticalAlign":"center", 
"TextHorizontalAlign":"left",
"Textbold":"yes"}');

label2 = Form1.AddNewProLabel(Form1, 'label2', 'KitApp');
clComponent.SetupComponent(label2, '{"Align":"Center", 
"MarginBottom":390,
"MarginLeft": 65,
"Width":170,
"Height":32,
"TextColor":"#333333", 
"TextSize":30, 
"TextVerticalAlign":"center", 
"TextHorizontalAlign":"left", 
"Textbold":"yes"}');


  BtnSifreSimgesi=Form1.AddNewProButton(Form1,'BtnSifreSimgesi','');
  BtnSifreSimgesi.Margins.Left= 280;
  BtnSifreSimgesi.Height=30;
  BtnSifreSimgesi.Width=35;
  BtnSifreSimgesi.Margins.bottom=30;
  BtnSifreSimgesi.clProSettings.BorderColor=clAlphaColor.clHexToColor('#f81c0d');
  BtnSifreSimgesi.clProSettings.BackGroundColor=clAlphaColor.clHexToColor('#ffffff');
  BtnSifreSimgesi.clProSettings.FontSize=20;
  BtnSifreSimgesi.clProSettings.RoundWidth=10;
  BtnSifreSimgesi.clProSettings.Roundheight=10;
  BtnSifreSimgesi.SetclProSettings(BtnSifreSimgesi.clProSettings);
  Form1.SetImage(BtnSifreSimgesi,'https://r.resimlink.com/HY-oB0.png');
  Form1.AddNewEvent(BtnSifreSimgesi,tbeOnClick,'Tiklandigindasifrebutonudegisme');

  
  GirisPaneli = Form1.AddNewProPanel(AnaPanel,'GirisPaneli');
  GirisPaneli.ALign = AlBottom;
  GirisPaneli.Height = AnaPanel.Height / 4;
  GirisPaneli.Margins.Top= 5;
  GirisPaneli.Margins.Left= 10;
  GirisPaneli.Margins.Left= 10;
  GirisPaneli.SetclProSettings(AnaPanel.clProSettings);
  
  BtnGiris = Form1.AddNewProButton(GirisPaneli,'BtnGiris','Log In'); 
  BtnGiris.Align=alLeft;
  BtnGiris.Width = GirisPaneli.Width / 2;
  BtnGiris.clTagStr = 'Giris';
  BtnGiris.clProSettings.TextSettings.Font.Style = [fsBold];
  BtnGiris.clProSettings.BorderColor = clAlphaColor.clHexToColor('#030303');
  BtnGiris.clProSettings.BorderWidth = 2;
  BtnGiris.clProSettings.RoundHeight = 20;
  BtnGiris.clProSettings.RoundWidth = 20;
  BtnGiris.clProSettings.IsRound = True;
  BtnGiris.SetclProSettings(BtnGiris.clProSettings);
  Form1.AddNewEvent(BtnGiris,tbeOnClick,'KullaniciKontrol');
  
  BtnKayitOl = Form1.AddNewProButton(GirisPaneli,'BtnKayitOl','Sign Up'); 
  BtnKayitOl.Align=alRight;
  BtnKayitOl.Width = GirisPaneli.Width / 2;
  BtnKayitOl.clTagStr = 'KayitOl';
  BtnKayitOl.SetclProSettings(BtnGiris.clProSettings);
  Form1.AddNewEvent(BtnKayitOl,tbeOnClick,'KullaniciKontrol');
  
  Form1.Run;
}

// uIslemler kodu


uses Main;
var
anaForm:TclStyleForm;
uIslemler:TclUnit;
Btn_KitapEkle,Btn_AddNote, Btn_KitapListe,Btn_Istatistikler,Btn_CikisYap: TclProButton;
plusImg,kitapImg,ListeImg,IstatistikImg,NoteImg,CikisImg,logoImg: TclProImage;
baslikLabel,label1:TClProLabel;
kullaniciAdi:String;

 void KitapEkleyineTiklandiginda;
 {
 Clomosy.RunUnit('uKitapEkle');
   }
 
void KitapListesineTiklandiginda;
 {
 Clomosy.RunUnit('uKitapListesi');
   }
 

 void IstatiklereTiklandiginda;
 {
 Clomosy.RunUnit('uIstatistikler');
  }
  
  void NotEkleyeTiklandiginda;
 {
 Clomosy.RunUnit('uNotEkle');
  }

void CikisYapTiklandiginda;
{
  Application.Terminate();  
}

void uIslemler_OnShow;
{
   baslikLabel.Text = 'Welcome, ' + Main.G_kullaniciAdi;
}
{
anaForm=TclStyleForm.Create(Self);
anaForm.SetFormColor('f0f9ff','', clGNone);



baslikLabel = anaForm.AddNewProLabel(anaForm, 'baslikLabel', 'Welcome, '+Main.G_kullaniciAdi + ' :)');
  clComponent.SetupComponent(baslikLabel, '{"Align": "Center",
  "MarginBottom": 590,
  "Width":300,
  "Height": 60,
  "TextColor": "#000000",
  "TextSize": 30,
  "TextVerticalAlign": "center",
  "TextHorizontalAlign": "left", 
  "TextItalic": "yes"}');


  Btn_KitapEkle=anaForm.AddNewProButton(anaForm, 'Btn_KitapEkle', '  Add A Book');
  Btn_KitapEkle.Align=alCenter;
  Btn_KitapEkle.Height=120;
  Btn_KitapEkle.Width=300;
  Btn_KitapEkle.Margins.bottom=350;
  Btn_KitapEkle.clProSettings.FontColor=clAlphaColor.clHexToColor('#000000');
  Btn_KitapEkle.clProSettings.BorderColor=clAlphaColor.clHexToColor('#3A86FF');
  Btn_KitapEkle.clProSettings.BackGroundColor=clAlphaColor.clHexToColor('#ffffff');
  Btn_KitapEkle.clProSettings.TextSettings.Font.Style = [fsBold];
  Btn_KitapEkle.clProSettings.BorderWidth = 2;
  Btn_KitapEkle.clProSettings.FontSize=20;
  Btn_KitapEkle.clProSettings.Roundheight=10;
  Btn_KitapEkle.clProSettings.RoundWidth=10;
  Btn_KitapEkle.SetclProSettings(Btn_KitapEkle.clProSettings);
   
   
  kitapImg= anaForm.AddNewProImage(anaForm,'kitapImg');
  kitapImg.Height =60;
  kitapImg.Width = 60;
  kitapImg.Margins.Right=190;
  kitapImg.Margins.bottom=350;
  anaForm.setImage(kitapImg,'https://d1nhio0ox7pgb.cloudfront.net/_img/v_collection_png/512x512/shadow/books_blue_add.png');
   
   
  Btn_KitapListe=anaForm.AddNewProButton(anaForm, 'Btn_KitapListe', 'Book List');
  Btn_KitapListe.Align=alCenter;
  Btn_KitapListe.Height=80;
  Btn_KitapListe.Width=300;
  Btn_KitapListe.Margins.bottom=90;
  Btn_KitapListe.clProSettings.FontColor=clAlphaColor.clHexToColor('#000000');
  Btn_KitapListe.clProSettings.BorderColor=clAlphaColor.clHexToColor('#4d996a');
  Btn_KitapListe.clProSettings.BackGroundColor=clAlphaColor.clHexToColor('#ffffff');
  Btn_KitapListe.clProSettings.TextSettings.Font.Style = [fsBold];
  Btn_KitapListe.clProSettings.BorderWidth = 2;
  Btn_KitapListe.clProSettings.FontSize=20;
  Btn_KitapListe.clProSettings.Roundheight=10;
  Btn_KitapListe.clProSettings.RoundWidth=10;
  Btn_KitapListe.SetclProSettings(Btn_KitapListe.clProSettings);
   
   
  ListeImg= anaForm.AddNewProImage(anaForm,'ListeImg');
  ListeImg.Height =60;
  ListeImg.Width = 60;
  ListeImg.Margins.Right=200;
  ListeImg.Margins.bottom=90;
  anaForm.setImage(ListeImg,'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSwX9wzhcgmYKrIIvY51QkJLHIS28Ntj1NE9g&s');
  
   
  Btn_Istatistikler=anaForm.AddNewProButton(anaForm, 'Btn_Istatistikler', '    Genre Statistics');
  Btn_Istatistikler.Align=alCenter;
  Btn_Istatistikler.Height=80;
  Btn_Istatistikler.Width=300;
  Btn_Istatistikler.Margins.top=130;
  Btn_Istatistikler.clProSettings.FontColor=clAlphaColor.clHexToColor('#000000');
  Btn_Istatistikler.clProSettings.BorderColor=clAlphaColor.clHexToColor('#8338EC');
  Btn_Istatistikler.clProSettings.BackGroundColor=clAlphaColor.clHexToColor('#ffffff');
  Btn_Istatistikler.clProSettings.TextSettings.Font.Style = [fsBold];
  Btn_Istatistikler.clProSettings.FontSize=20;
  Btn_Istatistikler.clProSettings.BorderWidth = 2;
  Btn_Istatistikler.clProSettings.Roundheight=10;
  Btn_Istatistikler.clProSettings.RoundWidth=10;
  Btn_Istatistikler.SetclProSettings(Btn_Istatistikler.clProSettings);
  Btn_Istatistikler.clProSettings.TextSettings.Font.Style = [fsBold];
  
  IstatistikImg= anaForm.AddNewProImage(anaForm,'IstatistikImg');
  IstatistikImg.Height =60;
  IstatistikImg.Width = 60;
  IstatistikImg.Margins.Right=200;
  IstatistikImg.Margins.top=130;
  anaForm.setImage(IstatistikImg,'https://cdn-icons-png.freepik.com/512/17223/17223335.png');

   
  
  Btn_AddNote=anaForm.AddNewProButton(anaForm, 'Btn_AddNote', 'Add Note');
  Btn_AddNote.Align=alCenter;
  Btn_AddNote.Height=80;
  Btn_AddNote.Width=300;
  Btn_AddNote.Margins.top=350;
  Btn_AddNote.clProSettings.FontColor=clAlphaColor.clHexToColor('#000000');
  Btn_AddNote.clProSettings.BorderColor=clAlphaColor.clHexToColor('#FF9F1C');
  Btn_AddNote.clProSettings.BackGroundColor=clAlphaColor.clHexToColor('#ffffff');
  Btn_AddNote.clProSettings.TextSettings.Font.Style = [fsBold];
  Btn_AddNote.clProSettings.BorderWidth = 2;
  Btn_AddNote.clProSettings.FontSize=20;
  Btn_AddNote.clProSettings.Roundheight=10;
  Btn_AddNote.clProSettings.RoundWidth=10;
  Btn_AddNote.SetclProSettings(Btn_AddNote.clProSettings);
  
  
  

  NoteImg= anaForm.AddNewProImage(anaForm,'NoteImg');
  NoteImg.Height =60;
  NoteImg.Width = 60;
  NoteImg.Margins.Right=190;
  NoteImg.Margins.top=350;
  anaForm.setImage(NoteImg,'https://cdn-icons-png.freepik.com/512/4439/4439796.png');

 
  Btn_CikisYap = anaForm.AddNewProButton(anaForm, 'Btn_CikisYap', '   Log Out');
  Btn_CikisYap.Align = alCenter;
  Btn_CikisYap.Height = 60;
  Btn_CikisYap.Width = 200;
  Btn_CikisYap.Margins.top = 520;
  Btn_CikisYap.clProSettings.FontColor = clAlphaColor.clHexToColor('#000000');
  Btn_CikisYap.clProSettings.BorderColor = clAlphaColor.clHexToColor('#EF476F');
  Btn_CikisYap.clProSettings.BackGroundColor = clAlphaColor.clHexToColor('#ffffff');
  Btn_CikisYap.clProSettings.TextSettings.Font.Style = [fsBold];
  Btn_CikisYap.clProSettings.BorderWidth = 2;
  Btn_CikisYap.clProSettings.FontSize = 20;
  Btn_CikisYap.clProSettings.Roundheight = 15;
  Btn_CikisYap.clProSettings.RoundWidth = 20;
  Btn_CikisYap.SetclProSettings(Btn_CikisYap.clProSettings);
  
  
  
  CikisImg= anaForm.AddNewProImage(anaForm,'CikisImg');
  CikisImg.Height =50;
  CikisImg.Width =50;
  CikisImg.Margins.left=50;
  CikisImg.Margins.Right=180;
  CikisImg.Margins.top=520;
  anaForm.setImage( CikisImg,'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS5jNcc8xPT07yj_4DXRWk8r0IXrkqNdSwFow&s');
  
  
 anaForm.AddNewEvent(Btn_CikisYap, tbeOnClick, 'CikisYapTiklandiginda'); 
 anaForm.AddNewEvent(Btn_KitapEkle,tbeOnClick,'KitapEkleyineTiklandiginda');
 anaForm.AddNewEvent( Btn_KitapListe,tbeOnClick,'KitapListesineTiklandiginda');
 anaForm.AddNewEvent(Btn_Istatistikler,tbeOnClick,'IstatiklereTiklandiginda');  
 anaForm.AddNewEvent(Btn_AddNote,tbeOnClick,'NotEkleyeTiklandiginda'); 
  
  anaForm.Run;
  }

///////////////////////////////////////////////////////////// uKitapEkle BÖLÜMÜ  /////////////////////////////////////////////////////////////////////////


Var   
 Form1:TClForm;
 RightLyt : TclLayout;
 PListView : TClProListView;
 DPanl : TClProListViewDesignerPanel;
 DeleteBtn,UpdateBtn,AlterBtn, SaveBtn,SilBtn: TClProButton;
 BookID,BookName,Author,Category,PageCount,ReadPage,Rating ,ratingLabel: TClProLabel;
 BookIDEdt,BookTitleEdt,AuthorEdt,CategoryEdt,StatusEdt,PageCountEdt : TclProEdit;
ratingStr: string;
alterImg,deleteImg,saveImg:TCLProImage;
i: Integer;
ratingComboBox: TCLComboBox;
 Void ClearEdtText
 {
   BookIDEdt.Text='';
   BookTitleEdt.Text='';
   AuthorEdt.Text='';
   CategoryEdt.Text='';
   StatusEdt.Text='';
   PageCountEdt.Text='';
   ratingComboBox.ItemIndex=-1;
 }
 
void GetData;
var
  Qry: TClSQLiteQuery;
{
  try
    Qry = Clomosy.DBSQLiteQueryWith('SELECT ''BookID: '' || BookID as BookID, ' +
                                     '''Book Name: '' || BookName as BookName, ' +
                                     '''Author: '' || Author as Author, ' +
                                     '''Category: '' || Category as Category, ' +
                                     '''ReadPage: '' || ReadPage as ReadPage, ' +
                                     '''Page Count: '' || PageCount as PageCount, ' +
                                     '''Rating: '' || Rating as Rating ' +
                                     'FROM Book_System');

    
    if (Qry == nil )
    {
      ShowMessage('HATA: Sorgu nesnesi (Qry) oluşturulamadı!');
      Exit;
    }

   
    Qry.OpenOrExecute;

    
    if not( Qry.Active )
    {
      ShowMessage('HATA: Sorgu çalıştırılamadı!');
      Exit;
   }

    
    if( Qry.Found )
   {
      if (PListView == nil )
     {
        ShowMessage('HATA: Liste bileşeni (PListView) bulunamadı!');
        Exit;
      }

      PListView.clLoadProListViewDataFromDataset(Qry);
    }
    else
   {
      PListView.Clearlist;
    }

  except
    ShowMessage('EXCEPTION: ' + LastExceptionClassName +
                ' | MESSAGE: ' + LastExceptionMessage);
  }
}

 
 Void DataInsert
 {
   if ((BookIDEdt.Text == '')||
   (BookTitleEdt.Text == '')||
   (AuthorEdt.Text == '')||
   (CategoryEdt.Text == '')||
   (StatusEdt.Text == '')||
   (PageCountEdt.Text == '')||
   (ratingComboBox.ItemIndex==-1))
   {
     ShowMessage('Fill in the Boxes...');
   }
   else
   {
     try
       Clomosy.DBSQLiteQuery.SQL.Text ='Insert Into Book_System(BookID,BookName,Author,Category,ReadPage,PageCount,Rating)Values('+QuotedStr(BookIDEdt.Text)+','
       +QuotedStr(BookTitleEdt.Text)+','+QuotedStr(AuthorEdt.Text)+','+QuotedStr(CategoryEdt.Text)+','+QuotedStr(StatusEdt.Text) +','+PageCountEdt.Text +','+QuotedStr(ratingComboBox.Items[ratingComboBox.ItemIndex])+')';
       Clomosy.DBSQLiteQuery.OpenOrExecute;
       GetData;
       ClearEdtText;
     except
      ShowMessage('Exception Class: '+LastExceptionClassName+' Exception GetData Message: '+LastExceptionMessage);
     }
   }
 }

 Void DataDelete
 {
   try
      if (BookIDEdt.Text <> '')
      {
      
     Clomosy.DBSQLiteQuery.SQL.Text = 'Delete From Book_System Where BookID = '+QuotedStr(BookIDEdt.Text);
     Clomosy.DBSQLiteQuery.OpenOrExecute;
     
     GetData;
     
     ClearEdtText;
     } else{
       ShowMessage('Please do not leave the Book ID field empty.');
     }
   except
      ShowMessage('Exception Class: '+LastExceptionClassName+' Exception GetData Message: '+LastExceptionMessage);
   }
 }
 
 Void DataUpdate
 {
   if ((BookIDEdt.Text == '')||(BookTitleEdt.Text == '')||(AuthorEdt.Text == '')||(CategoryEdt.Text == '')||(StatusEdt.Text == '')||(PageCountEdt.Text == '')|| (ratingComboBox.ItemIndex==-1))
   {
   
     ShowMessage('Fill in the Boxes...');
     
   }
   else
   {
     try
       Clomosy.DBSQLiteQuery.SQL.Text = 'Update Book_System Set BookID ='+QuotedStr(BookIDEdt.Text)+',BookName ='+QuotedStr(BookTitleEdt.Text)+
       ',Author ='+QuotedStr(AuthorEdt.Text)+',Category ='+QuotedStr(CategoryEdt.Text)+',ReadPage ='+QuotedStr(StatusEdt.Text)+
       ',PageCount ='+QuotedStr(PageCountEdt.Text)+',Rating='+QuotedStr(ratingComboBox.Items[ratingComboBox.ItemIndex])+'Where BookID ='+QuotedStr(BookIDEdt.Text);
       Clomosy.DBSQLiteQuery.OpenOrExecute;
       GetData;
       ClearEdtText;
     except
      ShowMessage('Exception Class: '+LastExceptionClassName+' Exception GetData Message: '+LastExceptionMessage);
     }
   }
 }

 
   void SqLiteConnectionCreateTable;
  var
    TableExists: Boolean;
  {
    try
      Clomosy.DBSQLiteConnect(Clomosy.AppFilesPath + 'library.db8', '');

      Clomosy.DBSQLiteQuery.Sql.Text = 'SELECT name FROM sqlite_master WHERE type="table" AND name="Book_System";';
      Clomosy.DBSQLiteQuery.OpenOrExecute;
      
      
      TableExists = not Clomosy.DBSQLiteQuery.Eof;
 
      if not (TableExists)
      {
        Clomosy.DBSQLiteQuery.Sql.Text = 'CREATE TABLE Book_System(BookID TEXT NOT NULL,
        BookName TEXT NOT NULL,
        Author TEXT NOT NULL,
        Category TEXT NOT NULL,
        ReadPage TEXT NOT NULL,
        PageCount TEXT NOT NULL,
        Rating TEXT NOT NULL)';
        Clomosy.DBSQLiteQuery.OpenOrExecute;
        
        ShowMessage('Table successfully added to the database!');
       
      } 

    except
     ShowMessage('Exception Class: '+LastExceptionClassName+' Exception SqLiteConnectionCreateTable Message: '+LastExceptionMessage);
    }
  }

void ShowAddBookForm
var
ratingValue:string;
{
  BookIDEdt = Form1.AddNewProEdit(RightLyt,'BookIDEdt','Book ID');
  BookIDEdt.Align = alMostTop;
  BookIDEdt.Height = RightLyt.Height/11-10;
  BookIDEdt.Margins.Top = 10;
  BookIDEdt.SetclProSettings(AlterBtn.clProSettings);
  BookIDEdt.clTypeOfField = taFloat;
  BookIDEdt.MaxLength = 10;

  BookTitleEdt = Form1.AddNewProEdit(RightLyt,'BookTitleEdt','Book Name');
  BookTitleEdt.Align = alMostTop;
  BookTitleEdt.Height = RightLyt.Height/11-10;
  BookTitleEdt.Margins.Top = 10;
  BookTitleEdt.SetclProSettings(AlterBtn.clProSettings);
  BookTitleEdt.MaxLength = 25;

  AuthorEdt = Form1.AddNewProEdit(RightLyt,'AuthorEdt','Author');
  AuthorEdt.Align = alMostTop;
  AuthorEdt.Height = RightLyt.Height/11-10;
  AuthorEdt.Margins.Top = 10;
  AuthorEdt.SetclProSettings(AlterBtn.clProSettings);
  AuthorEdt.MaxLength = 20;

  CategoryEdt = Form1.AddNewProEdit(RightLyt,'CategoryEdt','Category');
  CategoryEdt.Align = alMostTop;
  CategoryEdt.Height = RightLyt.Height/11-10;
  CategoryEdt.Margins.Top = 10;
  CategoryEdt.SetclProSettings(AlterBtn.clProSettings);
  CategoryEdt.MaxLength = 20;

  StatusEdt = Form1.AddNewProEdit(RightLyt,'StatusEdt','Read Page');
  StatusEdt.Align = alMostTop;
  StatusEdt.Height = RightLyt.Height/11-10;
  StatusEdt.Margins.Top = 10;
  StatusEdt.SetclProSettings(AlterBtn.clProSettings);
  StatusEdt.clTypeOfField = taFloat;
  StatusEdt.MaxLength = 3;

  PageCountEdt = Form1.AddNewProEdit(RightLyt,'PageCountEdt','Page Count');
  PageCountEdt.Align = alMostTop;
  PageCountEdt.Height = RightLyt.Height/11-10;
  PageCountEdt.Margins.Top = 10;
  PageCountEdt.SetclProSettings(AlterBtn.clProSettings);
  PageCountEdt.clTypeOfField = taFloat;
  PageCountEdt.MaxLength = 3;
  

  SaveBtn = Form1.AddNewProButton(RightLyt, 'SaveBtn', 'save');
  SaveBtn.Align = alMostTop;
  SaveBtn.Height = RightLyt.Height / 11 - 10;
  SaveBtn.Margins.Top = 20;
  SaveBtn.SetclProSettings(AlterBtn.clProSettings);
  Form1.AddNewEvent(SaveBtn, tbeOnClick, 'SaveAndHide');
  
  
  ratingComboBox=Form1.AddNewComboBox(RightLyt, 'ratingComboBox');
  ratingComboBox.Align =alMostTop;
  ratingComboBox.Height=RightLyt.Height/11-10;
  ratingComboBox.Margins.Top=30;

  ratingComboBox.Items.Add('1');
  ratingComboBox.Items.Add('2');
  ratingComboBox.Items.Add('3');
  ratingComboBox.Items.Add('4');
  ratingComboBox.Items.Add('5');
  ratingValue = ratingComboBox.Text;
 
  saveImg = Form1.AddNewImage(RightLyt, 'saveImg');
  saveImg.Height = 40;
  saveImg.Width = 40;
  saveImg.Margins.Right = 240;
  saveImg.Margins.bottom= 240;
  Form1.setImage(saveImg, 'https://icons.veryicon.com/png/o/miscellaneous/admin-dashboard-flat-multicolor/save-32.png');
   

  
  DPanl.Visible=False;
  alterImg.Visible=False;
  deleteImg.Visible=False;
  DeleteBtn.Visible = False;
  AlterBtn.Visible = False;
  PListView.Visible=False;
  
  
}

void SaveAndHide
{
  DataInsert; 
  
  Form1.RemoveComponent(BookIDEdt);
  Form1.RemoveComponent(BookTitleEdt);
  Form1.RemoveComponent(AuthorEdt);
  Form1.RemoveComponent(CategoryEdt);
  Form1.RemoveComponent(StatusEdt);
  Form1.RemoveComponent(PageCountEdt);
  Form1.RemoveComponent(SaveBtn); 
  Form1.RemoveComponent(saveImg);
  Form1.RemoveComponent(ratingComboBox);
 
  deleteImg.Visible=True;
  alterImg.Visible=True;
  DeleteBtn.Visible = True;
  AlterBtn.Visible = True;
  PListView.Visible=True;
 
  
  ratingComboBox.Visible=False;
  BookIDEdt.Visible = False;
  BookTitleEdt.Visible = False;
  AuthorEdt.Visible = False;
  CategoryEdt.Visible = False;
  StatusEdt.Visible = False;
  PageCountEdt.Visible = False;
  SaveBtn.Visible = False;
  saveImg.Visible=False;
}


void ShowDeleteBookForm
{
  BookIDEdt = Form1.AddNewProEdit(RightLyt,'BookIDEdt','Book ID');
  BookIDEdt.Align = alMostTop;
  BookIDEdt.Height = RightLyt.Height/11-10;
  BookIDEdt.Margins.Top = 10;
  BookIDEdt.SetclProSettings(DeleteBtn.clProSettings);
  BookIDEdt.clTypeOfField = taFloat;
  BookIDEdt.MaxLength = 10;
  
  SilBtn = Form1.AddNewProButton(RightLyt, 'SilBtn', 'Delete');
  SilBtn.Align = alMostTop;
  SilBtn.Height = RightLyt.Height / 11 - 10;
  SilBtn.Margins.Top = 20;
  SilBtn.SetclProSettings(DeleteBtn.clProSettings);
  Form1.AddNewEvent(SilBtn, tbeOnClick, 'DeleteAndClose');
  
 
  DeleteBtn.Visible = False;
  AlterBtn.Visible = False;
  PListView.Visible=False;
  alterImg.Visible=False;
  deleteImg.Visible=False;
}

void DeleteAndClose
{
  DataDelete; 
  Form1.RemoveComponent(BookIDEdt);
  Form1.RemoveComponent(SilBtn); 
  
  
  DeleteBtn.Visible = True;
  AlterBtn.Visible = True;
  PListView.Visible=True;
  deleteImg.Visible=True;
 alterImg.Visible=True;
  BookIDEdt.Visible = False;
  SilBtn.Visible = False;
}


void mainScreen;
{
  Form1 =TClForm.Create(Self);
  Form1.SetFormColor('#F2f6fc','',clGNone);
    
    
   PListView = Form1.AddNewProListView(Form1,'PListView');
   PListView.Align = alLeft;
   PListView.Width = Form1.clWidth;
   PListView.Margins.Right = 50;
   PListView.Margins.Left = 50;
   PListView.Margins.Bottom = 250;
   PListView.Margins.Left = 10;
   PListView.clProSettings.ViewType = lvWaterFall;
   PListView.clProSettings.IsRound = True;
   PListView.clProSettings.IsFill = True;
   PListView.clProSettings.ColCount = 1;
   PListView.clProSettings.ItemHeight = 150;
   PListView.clProSettings.ItemSpace = 5;
   PListView.clProSettings.BorderColor = clAlphaColor.clHexToColor('9e9e9e');
   PListView.clProSettings.BorderWidth = 5;
   PListView.SetclProSettings(PListView.clProSettings);
  

   DPanl = Form1.AddNewProListViewDesignerPanel(PListView,'DPanl'); 
   DPanl.Align = alTop;
   DPanl.Height = 80;
   DPanl.clProSettings.IsFill = True;
   DPanl.clProSettings.ItemHeight = 150;
   DPanl.clProSettings.BackgroundColor = clAlphaColor.clHexToColor('#E1F0FF');
   DPanl.clProSettings.BorderColor = clAlphaColor.clHexToColor('9e9e9e');
   DPanl.clProSettings.BorderWidth = 2;
   DPanl.SetclProSettings(DPanl.clProSettings);
   PListView.SetDesignerPanel(DPanl);

   
   
   BookID = Form1.AddNewProLabel(DPanl,'BookID','');
   BookID.Align = alMostTop;
   BookID.Margins.Top = 5;
   BookID.Margins.left = 7;
   BookID.Height = DPanl.Height/6;
   BookID.clProSettings.FontColor = clAlphaColor.clHexToColor('#060442');
  
   BookID.clProSettings.TextSettings.Font.Style = [fsBold];
    BookID.clProSettings.FontSize = 15;
   BookID.clProSettings.FontVertAlign = palcenter;
   BookID.clProSettings.FontHorzAlign = palLeading;
   BookID.clProSettings.RoundWidth = 10;
   BookID.clProSettings.RoundHeight = 10;
   BookID.SetclProSettings(BookID.clProSettings);
   DPanl.AddPanelObject(BookID,clCaption);
   
   BookName = Form1.AddNewProLabel(DPanl,'BookName','-');
   BookName.Align = alMostTop;
   BookName.Margins.Top = 5;
   BookName.Margins.left = 7;
   BookName.Height = DPanl.Height/5;
   BookName.clProSettings.TextSettings.Font.Style = [fsItalic];
   BookName.clProSettings.FontColor = clAlphaColor.clHexToColor('#060442');
   BookName.clProSettings.FontSize = 15;
   BookName.clProSettings.FontVertAlign = palcenter;
   BookName.clProSettings.FontHorzAlign = palLeading;
   BookName.clProSettings.RoundWidth = 10;
   BookName.clProSettings.RoundHeight = 10;
  BookName.SetclProSettings(BookName.clProSettings);
   DPanl.AddPanelObject(BookName,clText);
   
   Author = Form1.AddNewProLabel(DPanl,'Author','-');
   Author.Align = alMostTop;
   Author.Margins.Top = 5;
   Author.Margins.left = 7;
   Author.Height = DPanl.Height/5;
   Author.SetclProSettings(BookName.clProSettings);
   DPanl.AddPanelObject(Author,clText1);
   
   Category = Form1.AddNewProLabel(DPanl,'Category','-');
   Category.Align = alMostTop;
   Category.Margins.Top = 5;
   Category.Margins.left = 7;
   Category.Height = DPanl.Height/5;
   Category.SetclProSettings(BookName.clProSettings);
   DPanl.AddPanelObject(Category,clText2);
   
   ReadPage = Form1.AddNewProLabel(DPanl,'ReadPage','-');
   ReadPage.Align = alMostTop;
   ReadPage.Margins.Top = 5;
   ReadPage.Margins.left = 7;
   ReadPage.Height = DPanl.Height/5;
   ReadPage.SetclProSettings(BookName.clProSettings);
   DPanl.AddPanelObject(ReadPage,clText3);
   
   PageCount = Form1.AddNewProLabel(DPanl,'PageCount','-');
   PageCount.Align = alMostTop;
   PageCount.Margins.Top = 5;
   PageCount.Margins.left = 7;
   PageCount.Height = DPanl.Height/5;
   PageCount.SetclProSettings(BookName.clProSettings);
   DPanl.AddPanelObject(PageCount,clText4);
   
   Rating= Form1.AddNewProLabel(DPanl,'Rating','-');
   Rating.Align = alMostTop;
   Rating.Margins.Top = 5;
   Rating.Margins.left = 7;
   Rating.Height = DPanl.Height/5;
   Rating.SetclProSettings(BookName.clProSettings);
   DPanl.AddPanelObject(Rating,clText5);
   
   RightLyt = Form1.AddNewLayout(Form1,'RightLyt');
   RightLyt.Align=alRight;
   RightLyt.Margins.Right = 0; 
   RightLyt.Width = Form1.clWidth;
   
  DeleteBtn = Form1.AddNewProButton(RightLyt, 'DeleteBtn', 'Delete Book');
  DeleteBtn.Height = RightLyt.Height / 11 - 10;
  DeleteBtn.Width = Form1.clWidth;
  DeleteBtn.Margins.Top = 480;
  DeleteBtn.clProSettings.IsFill = True;
  DeleteBtn.clProSettings.BackgroundColor = clAlphaColor.clHexToColor('#f44336');
  DeleteBtn.clProSettings.FontColor = clAlphaColor.clHexToColor('#000000');
  DeleteBtn.clProSettings.BorderColor = clAlphaColor.clHexToColor('#000000');
  DeleteBtn.clProSettings.BorderWidth = 2;
  DeleteBtn.clProSettings.FontSize = 15;
  DeleteBtn.clProSettings.FontVertAlign = palcenter;
  DeleteBtn.clProSettings.FontHorzAlign = palCenter;
  DeleteBtn.clProSettings.TextSettings.Font.Style = [fsBold];
  DeleteBtn.SetclProSettings(DeleteBtn.clProSettings);
  Form1.AddNewEvent(DeleteBtn, tbeOnClick, 'ShowDeleteBookForm');


 deleteImg = Form1.AddNewImage(RightLyt, 'deleteImg');
  deleteImg.Height = 40;
  deleteImg.Width = 40;
  deleteImg.Margins.Right = 270;
  deleteImg.Margins.Top = 480;
  Form1.setImage(deleteImg, 'https://cdn-icons-png.flaticon.com/512/1345/1345823.png');
   

  AlterBtn = Form1.AddNewProButton(RightLyt, 'AlterBtn', 'Add Book');
  AlterBtn.Height = RightLyt.Height / 11 - 10;
  AlterBtn.Width = Form1.clWidth;
  AlterBtn.Margins.Top = 350;
  AlterBtn.clProSettings.IsFill = True;
  AlterBtn.clProSettings.BackgroundColor = clAlphaColor.clHexToColor('#1976d2');
  AlterBtn.clProSettings.FontColor = clAlphaColor.clHexToColor('#000000');
  AlterBtn.clProSettings.BorderColor = clAlphaColor.clHexToColor('#000000');
  AlterBtn.clProSettings.BorderWidth = 2;
  AlterBtn.clProSettings.FontSize = 15;
  AlterBtn.clProSettings.FontVertAlign = palcenter;
  AlterBtn.clProSettings.FontHorzAlign = palCenter;
  AlterBtn.clProSettings.TextSettings.Font.Style = [fsBold];
  AlterBtn.SetclProSettings(AlterBtn.clProSettings);
  Form1.AddNewEvent(AlterBtn, tbeOnClick, 'ShowAddBookForm');
 

  alterImg = Form1.AddNewImage(RightLyt, 'alterImg');
  alterImg.Height = 45;
  alterImg.Width = 45;
  alterImg.Margins.Right = 270;
  alterImg.Margins.Top = 350;
  Form1.setImage(alterImg, 'https://cdn0.iconfinder.com/data/icons/round-ui-icons/512/add_blue.png');
   
   Form1.Run;
}
 { 
   mainScreen;
   SqLiteConnectionCreateTable;
   GetData;
   
 }
 //////////////////////////////////////////////////// uKitap Listele BÖLÜMÜ ///////////////////////////////////////////////////////////////////////////

 Var
Form1:TClForm;
RightLyt : TclLayout;
PListView : TClProListView;
DPanl : TClProListViewDesignerPanel;
DeleteBtn,UpdateBtn,AlterBtn : TClProButton;
BookID,BookName,Author,Category,PageCount,Rating : TClProLabel;
BookIDEdt,BookTitleEdt,AuthorEdt,CategoryEdt,StatusEdt,PageCountEdt : TclProEdit;
testListview : TClProListView;
aramaEdt : TClProSearchEdit;
CategoryImg, AuthorImg, nameImg, CountImg:TClProImage;
ratingComboBox:TCLComboBox;
ratingStr:String;

Void ClearEdtText
{
BookIDEdt.Text='';
BookTitleEdt.Text='';
AuthorEdt.Text='';
CategoryEdt.Text='';
PageCountEdt.Text='';
ratingComboBox.ItemIndex=-1;
}

void GetData;
var
Qry : TClSQLiteQuery;
{

try
Qry = Clomosy.DBSQLiteQueryWith('Select ''BookID: ''||BookID as BookID,''''||BookName as BookName,''''||Author as Author, ''  ''||Category as Category,''Page Count: ''||PageCount as PageCount ,'' Rating : '' || Rating as Rating From Book_System');
Qry.OpenOrExecute;

if (Qry.Found)
{
PListView.clLoadProListViewDataFromDataset(Qry);
}
else
{
PListView.Clearlist;
}

except
ShowMessage('Exception Class: '+LastExceptionClassName+' Exception GetData Message: '+LastExceptionMessage);
}
}

Void onItemClicked;
{
ShowMessage(clGetStringAfter(PListView.clSelectedItemData(clCaption),': '));
BookIDEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clCaption),': ');
BookTitleEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText),': ');
AuthorEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText1),': ');
CategoryEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText2),': ');

PageCountEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText4),': ');
ratingStr=clGetStringAfter(PListView.clSelectedItemData(clText5),': ');
ratingComboBox.ItemIndex= ratingComboBox.Items.IndexOf(ratingStr);
}

void SqLiteConnectionCreateTable;
var
TableExists: Boolean;
{
try
Clomosy.DBSQLiteConnect(Clomosy.AppFilesPath + 'library.db8', '');


Clomosy.DBSQLiteQuery.Sql.Text = 'SELECT name FROM sqlite_master WHERE type="table" AND name="Book_System";';
Clomosy.DBSQLiteQuery.OpenOrExecute;


TableExists = not Clomosy.DBSQLiteQuery.Eof;


if not (TableExists)
{
Clomosy.DBSQLiteQuery.Sql.Text = 'CREATE TABLE Book_System(BookID TEXT NOT NULL,
BookName TEXT NOT NULL,
Author TEXT NOT NULL,
Category TEXT NOT NULL,
PageCount TEXT NOT NULL,
Rating TEXT NOT NULL )';
Clomosy.DBSQLiteQuery.OpenOrExecute;

ShowMessage('Table successfully added to the database!');

}

except
ShowMessage('Exception Class: '+LastExceptionClassName+' Exception SqLiteConnectionCreateTable Message: '+LastExceptionMessage);
}

}

void mainScreen;
{
Form1 =TClForm.Create(Self);
Form1.SetFormColor('#F0F9F4','',clGNone);

PListView = Form1.AddNewProListView(Form1,'PListView');

PListView.Align = alLeft;
PListView.Width = Form1.clWidth/(0.7);
PListView.Margins.Right = 10;
PListView.Margins.Bottom = 10;
PListView.Margins.Top= 20;
PListView.Margins.Left = 10;
PListView.clProSettings.ViewType = lvWaterFall;
PListView.clProSettings.IsRound = True;
PListView.clProSettings.IsFill = True;
PListView.clProSettings.ColCount = 1;
PListView.clProSettings.ItemHeight = 150;
PListView.clProSettings.ItemSpace = 5;
PListView.clProSettings.BorderColor = clAlphaColor.clHexToColor('#eddac0');
PListView.clProSettings.BorderWidth = 1;
PListView.clProSettings.RoundWidth = 3;
PListView.clProSettings.RoundHeight = 3;
PListView.SetclProSettings(PListView.clProSettings);

aramaEdt = Form1.AddNewProSearchEdit(Form1,'aramaEdt','Search...');
aramaEdt.Align = alTop;
aramaEdt.Margins.Top = 10;
aramaEdt.Margins.Left = 10;
aramaEdt.Margins.Right = 10;
aramaEdt.height=40;
aramaEdt.Width=Form1.clWidth;
aramaEdt.clProSettings.BorderColor = clAlphaColor.clHexToColor('#4d996a');
aramaEdt.clProSettings.BorderWidth = 2;
aramaEdt.clProSettings.RoundWidth = 10;
aramaEdt.clProSettings.RoundHeight = 10;
aramaEdt.clProSettings.FontSize = 15;
aramaEdt.clProSettings.IsRound = True;
aramaEdt.SetclProSettings(aramaEdt.clProSettings);
aramaEdt.TargetListView = PListView;

DPanl = Form1.AddNewProListViewDesignerPanel(PListView,'DPanl');
DPanl.Align = alTop;
DPanl.Margins.Top = 20;
DPanl.Height = 80;

DPanl.clProSettings.IsFill = true;
DPanl.clProSettings.ItemHeight = 150;
DPanl.clProSettings.BackgroundColor = clAlphaColor.clHexToColor('#E1F5E1');
DPanl.clProSettings.BorderColor = clAlphaColor.clHexToColor('#4d996a');
DPanl.clProSettings.IsRound = True;
DPanl.clProSettings.BorderWidth = 2;
DPanl.clProSettings.RoundWidth = 10;
DPanl.clProSettings.RoundHeight = 10;
DPanl.SetclProSettings(DPanl.clProSettings);
PListView.SetDesignerPanel(DPanl);
Form1.AddNewEvent(PListView,tbeOnItemClick,'onItemClicked');

nameImg= Form1.AddNewProImage(DPanl,'nameImg');
nameImg.Height =25;
nameImg.Width = 25;
nameImg.Margins.Right=500;
nameImg.Margins.bottom=43;
Form1.setImage(nameImg,' https://cdn-icons-png.flaticon.com/512/578/578666.png');

AuthorImg= Form1.AddNewProImage(DPanl,'AuthorImg');
AuthorImg.Height =25;
AuthorImg.Width = 25;
AuthorImg.Margins.Right=500;
AuthorImg.Margins.top=20;
Form1.setImage(AuthorImg,'https://cdn-icons-png.flaticon.com/512/3252/3252812.png');



CountImg= Form1.AddNewProImage(DPanl,'CountImg');
CountImg.Height =25;
CountImg.Width = 25;
CountImg.Margins.Right=500;
CountImg.Margins.top=80;
Form1.setImage(CountImg,'https://cdn-icons-png.flaticon.com/512/11092/11092456.png');

BookID = Form1.AddNewProLabel(DPanl,'BookID','-');
BookID.Margins.bottom=100;
BookID.Margins.right = 320;
BookID.Height = DPanl.Height/5;
BookID.clProSettings.FontColor = clAlphaColor.clHexToColor('#0c0d0d');
BookID.clProSettings.FontSize = 15;
BookID.clProSettings.TextSettings.Font.Style = [fsBold];
BookID.clProSettings.RoundWidth = 10;
BookID.clProSettings.RoundHeight = 10;
BookID.SetclProSettings(BookID.clProSettings);
DPanl.AddPanelObject(BookID,clCaption);

BookName = Form1.AddNewProLabel(DPanl,'BookName','-');
BookName.Margins.bottom = 40;
BookName.Margins.right = 320;
BookName.Height = DPanl.Height/4;
BookName.SetclProSettings(BookID.clProSettings);
DPanl.AddPanelObject(BookName,clText);

Author = Form1.AddNewProLabel(DPanl,'Author','-');
Author.Margins.top = 20;
Author.Margins.right = 320;
Author.Height = DPanl.Height/4;
Author.SetclProSettings(BookID.clProSettings);
DPanl.AddPanelObject(Author,clText1);

Category = Form1.AddNewProLabel(DPanl,'Category','-');
Category.Margins.bottom = 40;
Category.Margins.left = 40;
Category.Height = DPanl.Height/4;
Category.clProSettings.BackgroundColor = clAlphaColor.clHexToColor('#4d996a');
Category.clProSettings.FontColor=clAlphaColor.clHexToColor('#000000');
Category.clProSettings.RoundWidth = 7;
Category.clProSettings.RoundHeight = 7;
Category.SetclProSettings(Category.clProSettings);
DPanl.AddPanelObject(Category,clText2);


Rating= Form1.AddNewProLabel(DPanl,'Rating','-');
Rating.Margins.Top = 80;
Rating.Margins.left = 40;
Rating.clProSettings.FontHorzAlign = palTrailing;
Rating.clProSettings.FontVertAlign = palCenter;
Rating.Height = DPanl.Height/4;
Rating.SetclProSettings(BookID.clProSettings);
DPanl.AddPanelObject(Rating,clText5)

PageCount = Form1.AddNewProLabel(DPanl,'PageCount','-');
PageCount.Margins.Top = 80;
PageCount.Margins.Right = 320;
PageCount.Height = DPanl.Height/4;
PageCount.SetclProSettings(BookID.clProSettings);
DPanl.AddPanelObject(PageCount,clText4);


Form1.Run;
}
{
mainScreen;

SqLiteConnectionCreateTable;
GetData;

}

 ///////////////////////////////////////////////////// uIstatistikler BÖLÜMÜ  /////////////////////////////////////////////////////////////////


 
var
  Form1:TCLForm;
  clPageControl:TCLPageControl;
  arrPageName : TclArrayString;
  BookIDEdt,BookTitleEdt,AuthorEdt,CategoryEdt,StatusEdt,PageCountEdt : TclProEdit;
  Chart1: TClChart;
  PListView: TClProListView;
  I : Integer;
  chartPanel: TClContainer;
chartTitleLbl: TclProLabel;
void GetData;
 var
  Qry : TClSQLiteQuery;
 {
  
  try
    Qry = Clomosy.DBSQLiteQueryWith('Select ''BookID: ''||BookID as BookID,''Book Name: ''||BookName as BookName,''Author: ''||Author as Author, ''Category: ''||Category as Category,''Read Page ''||ReadPage as ReadPage,''Page Count: ''||PageCount as PageCount From Book_System');
    Qry.OpenOrExecute;
    
    if (Qry.Found)
    {
     PListView.clLoadProListViewDataFromDataset(Qry);
    }
    else
    {
      PListView.Clearlist;
    }
  except
    ShowMessage('Exception Class: '+LastExceptionClassName+' Exception GetData Message: '+LastExceptionMessage);
  }      
 }


 Void onItemClicked
 {
   ShowMessage(clGetStringAfter(PListView.clSelectedItemData(clCaption),': '));
   BookIDEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clCaption),': ');
   BookTitleEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText),': ');
   AuthorEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText1),': ');
   CategoryEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText2),': ');
   StatusEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText3),': ');
   PageCountEdt.Text=clGetStringAfter(PListView.clSelectedItemData(clText4),': ');
 }
 
 

  void SqLiteConnectionCreateTable;
  var
    TableCategoryStatistics: Boolean;
    QCategoryStatistics, QCustomerAnalytics, QRegionalDistribution :TClSQLiteQuery;
  {
    try
      Clomosy.DBSQLiteConnect(Clomosy.AppFilesPath + 'library.db8', '');

      QCategoryStatistics = Clomosy.DBSQLiteQueryWith('SELECT name FROM sqlite_master WHERE type="table" AND name="CategoryStatistics";');
      QCategoryStatistics.OpenOrExecute;

      TableCategoryStatistics = not QCategoryStatistics.Eof;
  
      
      if not (TableCategoryStatistics )
      {
        Clomosy.DBSQLiteQuery.Sql.Text = 'CREATE TABLE CategoryStatistics (
                                          ID INTEGER PRIMARY KEY AUTOINCREMENT,
                                          Categories TEXT NOT NULL,
                                          ReadBook INTEGER NOT NULL
                                          );';
        Clomosy.DBSQLiteQuery.OpenOrExecute;
    
        
        ShowMessage('Table successfully added to the database!');
        
      }
      

    except
     ShowMessage('Exception Class: '+LastExceptionClassName+' Exception Message: '+LastExceptionMessage);
    }
  }
  
void AddDataToChart;
Var
  QryCategoryStatistics: TClSQLiteQuery;
{
  try
   QryCategoryStatistics = Clomosy.DBSQLiteQueryWith(
  'SELECT LOWER(TRIM(Category)) AS Genre, ' +
  'COUNT(*) AS TotalReadBooks, ' +
  'CASE ' +
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('dram') + ' THEN ' + QuotedStr('clPink') + ' ' +
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('polisiye') + ' THEN ' + QuotedStr('clblue') + ' ' +
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('romantik') + ' THEN ' + QuotedStr('clred') + ' ' +
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('fantastik') + ' THEN ' + QuotedStr('clPurple') + ' ' +
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('kişisel gelişim') + ' THEN ' + QuotedStr('clGreen') + ' ' + 
     'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('roman') + ' THEN ' + QuotedStr('clYellow') + ' ' +
     'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('korku') + ' THEN ' + QuotedStr('clPeru') + ' ' + 
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('bilim kurgu') + ' THEN ' + QuotedStr('clYellowGreen') + ' ' + 
    'WHEN LOWER(TRIM(Category)) = ' + QuotedStr('psikoloji') + ' THEN ' + QuotedStr('clOrange') + ' ' + 
    'ELSE ' + QuotedStr('clGray') + ' ' +
  'END AS color ' +
  'FROM Book_System ' +
  'GROUP BY LOWER(TRIM(Category));');

    QryCategoryStatistics.OpenOrExecute;
    
      
    
    
    
    
    if (QryCategoryStatistics.Found)
    {
     Chart1 = Form1.AddNewChart(clPageControl.PageContainers[0], 'Chart1', 'Clomosy Inc');
      Chart1.Height = 250;
      
      Chart1.Margins.bottom=300;
      Chart1.Width = 430 ;
      Chart1.ChartType = clCSizedPie;
     
      Chart1.XAxisText = 'Genre';
      Chart1.ChartItemText = 'Genre';
      Chart1.ChartItemsValue = 'TotalReadBooks';
      Chart1.ChartTitle = 'Number of Books Read per Genre';
      Chart1.clLoadDataFromDataSet(QryCategoryStatistics);
    }
  except
    ShowMessage('Exception class: ' + LastExceptionClassName + ' Exception Message: ' + LastExceptionMessage);
  }
}
 void AddRatingColumnChart;
var
  QryRatingStatistics: TClSQLiteQuery;
  ChartRating: TClChart;
{
  try
   
    
   QryRatingStatistics = Clomosy.DBSQLiteQueryWith(
  'SELECT ' +
  '  R.Rating, ' +
  '  IFNULL(B.BookCount, 0) AS BookCount, ' +
  '  CASE ' +
  '    WHEN R.Rating = 1 THEN ' + QuotedStr('clRed') + ' ' +
  '    WHEN R.Rating = 2 THEN ' + QuotedStr('clOrange') + ' ' +
  '    WHEN R.Rating = 3 THEN ' + QuotedStr('clYellow') + ' ' +
  '    WHEN R.Rating = 4 THEN ' + QuotedStr('clBlue') + ' ' +
  '    WHEN R.Rating = 5 THEN ' + QuotedStr('clGreen') + ' ' +
  '    ELSE ' + QuotedStr('clGray') + ' ' +
  '  END AS color, ' +
  '  CAST(R.Rating AS TEXT) AS RatingText ' +
  'FROM ' +
  '  (SELECT 1 AS Rating UNION ALL ' +
  '   SELECT 2 UNION ALL ' +
  '   SELECT 3 UNION ALL ' +
  '   SELECT 4 UNION ALL ' +
  '   SELECT 5) AS R ' +
  'LEFT JOIN ' +
  '  (SELECT Rating, COUNT(*) AS BookCount FROM Book_System WHERE Rating BETWEEN 1 AND 5 GROUP BY Rating) AS B ' +
  'ON R.Rating = B.Rating ' +
  'ORDER BY R.Rating;'
);
    QryRatingStatistics.OpenOrExecute;
    
     
    if (QryRatingStatistics.Found)
    {
      ChartRating = Form1.AddNewChart(clPageControl.PageContainers[0], 'ChartRating', 'RatingChart');
      
      ChartRating.Width = Form1.clWidth - 50;
      ChartRating.Height = 250;
      // Eğer başka grafik varsa onun altına yerleştir
       ChartRating.Margins.top=250;
      ChartRating.ChartType = clCBar; // Sütun grafik
      
      ChartRating.XAxisText = 'RatingText';
      ChartRating.ChartItemText = 'RatingText';
      ChartRating.ChartItemsValue = 'BookCount';
      ChartRating.ChartTitle = 'Number of Books by Rating (1-5)';
      
      ChartRating.clLoadDataFromDataSet(QryRatingStatistics);
    }
  except
    ShowMessage('Exception class: ' + LastExceptionClassName + ' Exception Message: ' + LastExceptionMessage);
  }
}
  
{
  Form1 = TCLForm.Create(Self);
  Form1.SetFormColor('#8338EC', '', clGNone);

 arrPageName = TclArrayString.Create;
  arrPageName = ['Book Static'];
  clPageControl = Form1.AddNewPageControl(Form1,'clPageControl');
  clPageControl.Align = alClient;
  clPageControl.Pages[0].Text = arrPageName[0];
  clPageControl.Tabsize.Mode = tsmFixedSizeAutoShrink;
  clPageControl.Tabsize.Width =Form1.clWidth;
  clPageControl.Tabsize.Height = 0;
 
chartTitleLbl = Form1.AddNewProLabel(clPageControl.PageContainers[0], 'chartTitleLbl', 'Book Statistics');
chartTitleLbl.Align = alTop;
chartTitleLbl.Margins.Top = 40; 
chartTitleLbl.Height = 25;
chartTitleLbl.clProSettings.FontSize = 18;
chartTitleLbl.clProSettings.TextSettings.Font.Style = [fsBold];
chartTitleLbl.clProSettings.FontHorzAlign = palCenter;
chartTitleLbl.SetclProSettings(chartTitleLbl.clProSettings);
  
  SqLiteConnectionCreateTable;
  AddDataToChart;
  AddRatingColumnChart;
  Form1.Run;
  
}
      //////////////////////////////////////////////////////////// uNotEkle BÖLÜMÜ ///////////////////////////////////////////////////

      Var  
  Form1:TclForm;
  FileStr : String;
  strList: TclStringList;
  topLyt, btnLyt, bottomLyt,btnDeleteLyt : TclLayout;
  noteEdt : TclProEdit;
  addBtn,deleteBtn : TclProButton;
  noteAddLbl, notesLbl :TclProLabel;
  mainPanel : TclProPanel;
  noteMemo : TclMemo;

void BtnOnClick;
var
  nowDateTime: TclDateTime;
{
  nowDateTime = Now;
  noteMemo.Lines.Add(FormatDateTime('dd.mm.yyyy hh:nn:  ', nowDateTime) + noteEdt.Text);
  strList.Add(FormatDateTime('dd.mm.yyyy hh:nn:  ', nowDateTime) + noteEdt.Text);
  noteEdt.Text = '';
  strList.SaveToFile(FileStr, 0);
}

void BtnOnDelete;
{
  strList.Clear;
  noteMemo.Lines.Clear;
  strList.SaveToFile(FileStr, 0);
}

{
  Form1=TclForm.Create(self);
  strList = Clomosy.StringListNew;
  FileStr = clPathCombine('Task.Txt', Clomosy.AppFilesPath);
Form1.SetFormColor('#F6E27F','',clGNone);
  
  mainPanel=Form1.AddNewProPanel(Form1,'mainPanel');
  mainPanel.Align = alClient;
  mainPanel.Margins.Bottom = 5;
  mainPanel.Margins.Top = 5;
  mainPanel.Margins.Right = 5;
  mainPanel.Margins.Left = 5;
  mainPanel.clProSettings.BorderColor = clAlphaColor.clHexToColor('#000000');
  mainPanel.clProSettings.BackgroundColor = clAlphaColor.clHexToColor('#F6E27F');
  mainPanel.clProSettings.BorderWidth = 1;
  mainPanel.clProSettings.IsRound = True;
  mainPanel.clProSettings.RoundHeight = 10;
  mainPanel.SetclProSettings(mainPanel.clProSettings);
  
  topLyt = Form1.AddNewLayout(mainPanel,'topLyt');
  topLyt.Align=ALMostTop;
  topLyt.Height = 150;
  
  
  noteAddLbl = Form1.AddNewProLabel(topLyt,'noteAddLbl','Add Note');
  noteAddLbl.Align = alMostTop;
  noteAddLbl.Height = 20;
  noteAddLbl.Margins.Top = 20;
  noteAddLbl.Margins.Left = 10;
  noteAddLbl.clProSettings.FontColor = clAlphaColor.clHexToColor('#030303');
  noteAddLbl.clProSettings.FontSize = 15;
  noteAddLbl.clProSettings.TextSettings.Font.Style = [fsBold];
  noteAddLbl.clProSettings.FontVertAlign = palcenter;//palLeading , palCenter , palTrailing
  noteAddLbl.clProSettings.FontHorzAlign = palLeading;
  noteAddLbl.SetclProSettings(noteAddLbl.clProSettings);
  
  noteEdt = Form1.AddNewProEdit(topLyt,'noteEdt','Enter your note...');
  noteEdt.Align = alTop;
  noteEdt.Height = 45;
  noteEdt.clProSettings.IsRound = True;
  noteEdt.clProSettings.RoundHeight = 10;
  noteEdt.clProSettings.RoundWidth = 10;
  noteEdt.Margins.Top = 10;
  noteEdt.Margins.Left = 10;
  noteEdt.Margins.Right = 10;
  noteEdt.clProSettings.BorderColor = clAlphaColor.clHexToColor('#000000');
  noteEdt.clProSettings.BorderWidth = 1;
  noteEdt.SetclProSettings(noteEdt.clProSettings);
  
  btnLyt = Form1.AddNewLayout(topLyt,'btnLyt');
  btnLyt.Align=ALBottom;
  btnLyt.Height = 50;
  
  addBtn = Form1.AddNewProButton(btnLyt,'addBtn','Save');
  addBtn.Align = AlRight;
  addBtn.Width = 70;
  addBtn.Margins.Right = 10;
 
 
  Form1.AddNewEvent(addBtn,tbeOnClick,'BtnOnClick');
  
  bottomLyt = Form1.AddNewLayout(mainPanel,'bottomLyt');
  bottomLyt.Align=ALTop;
 
  bottomLyt.Height = 100;
  bottomLyt.Margins.Top = 20;
  
  notesLbl = Form1.AddNewProLabel(bottomLyt,'notesLbl','Notes');
  notesLbl.Align = alMostTop;
  notesLbl.Height = 20;
  notesLbl.Margins.Top = 10;
  notesLbl.Margins.Left = 20;
  notesLbl.clProSettings.FontColor = clAlphaColor.clHexToColor('#030303');;
  notesLbl.clProSettings.FontSize = 15;
  notesLbl.clProSettings.TextSettings.Font.Style = [fsBold];
  notesLbl.clProSettings.FontVertAlign = palcenter;//palLeading , palCenter , palTrailing
  notesLbl.clProSettings.FontHorzAlign = palLeading;
  notesLbl.SetclProSettings(notesLbl.clProSettings);
  
  noteMemo = Form1.AddNewMemo(bottomLyt,'noteMemo','');
  noteMemo.Align = alTop;
  noteMemo.Height = 350;
  noteMemo.Width = 150;
  noteMemo.Margins.Left= 10;
  noteMemo.Margins.Right= 10; 
  noteMemo.Margins.Top= 10;
  noteMemo.ReadOnly = True;
  noteMemo.TextSettings.WordWrap = True;
  
  btnDeleteLyt = Form1.AddNewLayout(mainPanel,'btnDeleteLyt');
  btnDeleteLyt.Align=alMostTop;
  btnDeleteLyt.Height = 50;
  btnDeleteLyt.Margins.Top = 5;
  btnDeleteLyt.Margins.Bottom = 60;
  btnDeleteLyt.Margins.Right = 10;
  
  deleteBtn = Form1.AddNewProButton(btnDeleteLyt,'deleteBtn','Delete');
  deleteBtn.ALign = AlRight;
  deleteBtn.Width = 70;
  deleteBtn.Margins.Left = 10;
 
  Form1.AddNewEvent(deleteBtn,tbeOnClick,'BtnOnDelete');
  
  if clFileExists('Task.Txt', Clomosy.AppFilesPath) 
  {
    strList.LoadFromFile(FileStr, 0);
    noteMemo.Lines.Assign(strList);
  }
  
  Form1.Run;
}

