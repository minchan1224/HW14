# HW14

(클라이언트 코드)

import java.net.*;
import java.io.*;

Socket socket;
PrintWriter out;
BufferedReader in;

void setup() {
  size(400, 400);
  String serverAddress = "127.0.0.1";  // 서버 주소 (localhost)
  int port = 12345;
  
  try {
    // 서버에 연결
    socket = new Socket(serverAddress, port);
    println("서버에 연결됨");

    // 서버와의 입출력 스트림 설정
    out = new PrintWriter(socket.getOutputStream(), true);
    in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
    
  } catch (IOException e) {
    e.printStackTrace();
  }
}

void draw() {
  background(255);
  fill(0);
  textAlign(CENTER, CENTER);
  text("클라이언트 대기 중...", width / 2, height / 2);

  // 서버로부터 메시지를 받으면 출력
  if (in != null) {
    try {
      if (in.ready()) {
        String serverMessage = in.readLine();
        if (serverMessage != null) {
          println("서버로부터 받은 메시지: " + serverMessage);
        }
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}

// 서버에 메시지 전송
void sendMessage(String message) {
  if (out != null) {
    out.println(message);
    println("클라이언트 -> 서버: " + message);
  }
}

void keyPressed() {
  if (key == 'c') {
    sendMessage("안녕하세요 3조 입니다");
  }
}
