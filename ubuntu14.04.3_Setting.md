우분투 설치 후 초기 설정 
==========================================================

####소프트웨어 업데이트 설정
- 시스템 설정 -> 소프트웨어 & 업데이트
        Ubuntu 소프트웨어 -> 다운로드 위치 
            : 기타 -> 대한민국, ftp.daum.net 선택

![다운로드](https://github.com/hongu/etc/blob/master/01_Download_Server.jpg?raw=true)


####우분투 한글 언어팩 설치      

- 영문 전환이 안되는 경우,   
        시스템 설정 -> 텍스트 입력창 설정
            + 눌러 입력 소스 추가, 영어(미국) 선택, 수동으로 한영전환 
                ( 언어팩 설치후 삭제 )

![입력소스](https://github.com/hongu/etc/blob/master/02_lang_01.jpg?raw=true)

+ UIM 벼루 설치
   Ctrl + Alt + t 를 눌러서 터미널 창을 연다. 
 
```
$ sudo apt-get update
$ sudo apt-get install uim uim-byeoru
```

- 설치후, 시스템 설정 -> 언어지원 
    언어지원 패키지 설치 메세지가 나오면 설치, 
        키보드 입력기를 : uim 으로 변경 

![다운로드](https://github.com/hongu/etc/blob/master/02_lang_02.jpg?raw=true)
        
 - 설정 후, 재부팅 ....      
    좌측 상단의 언어 아이콘을 클릭하여 hangul 선택 한 후, shift + space로 한영 전환이 되면 정상

![한글](https://github.com/hongu/etc/blob/master/02_lang_03.jpg?raw=true)


#### VMware Tools 설치 
 
https://www.youtube.com/watch?v=f0VNQ7_LOAY
-> 링크 참조 

- VMware Tools 다운로드 
    VMware 메뉴 -> Player -> Manage -> Install VMware Tools (한번 설치된 상태면 Reinstall.... 이라고 뜸) 다운로드 파일을 열어서 바탕화면에 압축을 푼다.

![VMware](https://github.com/hongu/etc/blob/master/03_VMware_01.jpg?raw=true)
![VMware](https://github.com/hongu/etc/blob/master/03_VMware_02.jpg?raw=true)
    
+ 터미널을 열어 슈퍼유저(root) 권한으로 설치 
+ Ctrl + Alt + T 눌러 터미널 창을 열고, sudo su 명령을 써서  root 계정으로 로그인 한다. 
+ 압축을 푼 폴더의 VMware-install.pl 파일을 드래그 하여, 터미널에 놓으면 아래와 같이 입력 이되고, 이를 실행한다.

```
$ sudo su
[sudo] password for ........... : 
$ root@.............# '/................./바탕화면/vmware-tools-distrib/vmware-install.pl'
```
![VMware](https://github.com/hongu/etc/blob/master/03_VMware_03.jpg?raw=true)

+ VMware 내부 디스플레이 확장이라 던지, 마우스 드래그 복사 같은 편의 기능이 지원됨, 취향에 따라 알아서 해석해서 깔자, 귀찮으면 Yes! yEs! yeS!
+ 설치가 종료 되면, 재부팅 ...... 
![VMware](https://github.com/hongu/etc/blob/master/03_VMware_04.jpg?raw=true)


#### Unity Tweak Tools 설치 

- 테마나 폰트 같은 인터페이스 설정용 툴 
```
$ sudo apt-get install fonts-inconsolata    // 현재 쓰고 있는 폰트로 필요하면 설치 

$ sudo apt-get install unity-tweak-tool
```

![TWEAK](https://github.com/hongu/etc/blob/master/04_Unity_Tweak_02.jpg?raw=true)
  

#### VIM 설치 및 설정 

- VIM 설치 
```
$ sudo apt-get install vim
```

- VIM 설정 
```
$ vi ~/.vimrc
    i 눌러서 입력 모드 진입, 아래의 내용을 복사 붙여넣기 
```
```
syntax on
filetype on
set nocompatible
set laststatus=2
set statusline=%h%F%m%r%=[%l:%c(%p%%)]
set ts=4
set nonu
set autoindent
set cindent
set shiftwidth=4

set background=dark
set backspace=eol,start,indent
set history=1000
set fileencodings=utf-8,euc-kr

autocmd BufReadPost *
    \ if line("'\"") > 0 && line("'\"") <= line("$") |
    \   exe "normal g`\"" |
    \ endif

map <F3> <c-w><c-w>
map <F4> :Tlist<cr>
map <F5> :BufExplorer<cr>
map <F6> <esc> :w<cr> :make<cr> :ccl<cr> :cw<cr>
```
    ESC 키를 눌러 입력 모드를 빠져나오고
    :wq ↵  저장 종료 



#### 티미널 프롬프트 설정 

```
$ vi ~/.bashrc
        VIM 설정 할때 처럼, 아래 내용 복사 붙여넣기 저장 
```
```
export PS1='\[\033[32;40m\]\u\[\033[32m\]@\h \[\033[33;40m\w\033[0m\]\n\$ '
export PROMPT_COMMAND=$'echo -ne "\\033]0;${PWD}\\007"'
```


#### 개발 관련 패키지 설치 

- 임베디드 개발 환경 패키지로 ..... 필요한 부분만 설치..... 귀찮으면 그냥 다 설치 
```
$ sudo apt-get install build-essential subversion ccache sed wget cvs git-core coreutils unzip texinfo docbook-utils gawk help2man diffstat file g++ texi2html bison flex htmldoc chrpath libxext-dev
$ sudo apt-get install doxygen corkscrew minicom vim xinetd tftp tftpd tree
$ sudo apt-get install kpartx
$ sudo apt-get install libsdl1.2-dev
```

- 64bit Ubuntu에서 32bit 라이브러리 사용을 위한 패키지 
```
$ sudo apt-get install libc6:i386 libx11-6:i386 libasound2:i386 libatk1.0-0:i386 libcairo2:i386 libcups2:i386 libdbus-glib-1-2:i386 libgconf-2-4:i386 libgdk-pixbuf2.0-0:i386 libgtk-3-0:i386 libice6:i386 libncurses5:i386 libsm6:i386 liborbit2:i386 libudev1:i386 libusb-0.1-4:i386 libstdc++6:i386 libxt6:i386 libxtst6:i386 libgnomeui-0:i386 libusb-1.0-0-dev:i386 libcanberra-gtk-module:i386 gtk2-engines-murrine:i386
```

- gtk 관련 개발 라이브러리 
```
$ sudo apt-get install libgtk2.0-dev 
$ sudo apt-get install libgtk-3-dev
```
- 나머지는 필요할때 마다 설치하자 


#### sublime-text3

- 홈페이지에서 설치 파일 다운로드 
https://www.sublimetext.com/

- 터미널에서 아래와 같이 입력 하거나 설치 파일을 더블클릭하면 자동으로트 소프트웨어 센터와 연동되어 설치됨 
```
        
$ dpkg -i "설치 파일명"       
        //  다운로드 위치에서
$ subl
        // sublime 이라고 검색해서 실행해도됨 
```

- Package Control 설치 ( Plug-in 설치툴 )
   Ctrl + ` 눌러 파이썬 콘솔을 열고 아래와 같이 입력 
```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

- Package Control 실행 및 Plug-in 설치


    + 추천 패키지 
        https://packagecontrol.io/    

    ***ARM Assembly***
    ***AngularJS***
    ***SublimeCodeIntel***
    ***SidebarEnhancements***
    ***DocBlockr***
    ***Bracket Highlighter***
    ***Emmet***
    ***Git***
    ***Gist***
    ***Markdown Editing***
    ***Markdown Preview***
    ***SmartMarkdown***
    ***Nodejs***
    ***JSLint***
    ***SublimeLinter***
    ***Terminal***
    ***ConvertToUTF8***
    ***Codec33***
    ***IMEsupport*** --- 리눅스에서는 설치되지 않음 

- 사용자 설정 
    Preferences -> Setting - User 
```
{
            "always_show_minimap_viewport": true,
            "bold_folder_labels": true,
            "caret_style": "phase",
            "color_scheme": "Packages/Color Scheme - Default/Monokai.tmTheme",
            "default_encoding": "UTF-8",
            "draw_minimap_border": true,
            "font_face": "Inconsolata Medium",
            "font_size": 13,
            "highlight_line": true,
            "highlight_modified_tabs": true,
            "ignored_packages":
            [
                "BracketHighlighter",
                "SideBarEnhancements",
                "Vintage"
            ],
            "indent_guide_options":
            [
                "draw_normal",
                "draw_active"
            ],
            "soda_classic_tabs": true,
            "soda_folder_icons": true,
            "tab_size": 3,
            "theme": "Soda Dark.sublime-theme",
            "trim_trailing_white_space_on_save": true,
            "word_wrap": true
}
```
