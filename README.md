graph TD
    User["사용자"] -->|실행| App("App (app.py)")
    
    subgraph "메인 프로세스 (GUI 및 제어)"
        App --> MainWin["메인 윈도우 UI<br/>(Tkinter)"]
        MainWin -->|시작 버튼| AudioProc["오디오 프로세서<br/>(AudioProcessor)"]
        MainWin -->|설정 변경| WSServer["웹소켓 서버<br/>(WebSocketServer)"]
    end
    
    subgraph "오디오 처리 스레드 (백그라운드)"
        Mic["마이크 입력"] -->|Audio Data| AudioProc
        AudioProc -->|VAD 감지| Whisper["Whisper AI<br/>(STT)"]
        Whisper -->|텍스트| Translator["번역기<br/>(Google/DeepL)"]
        Translator -->|번역 결과| WSServer
    end
    
    subgraph "오버레이 프로세스 (별도 프로세스)"
        WSServer -->|JSON 데이터 전송| OverlayWin["오버레이 창<br/>(pywebview)"]
        OverlayWin -->|HTML/JS 렌더링| Screen["화면 출력"]
    end
