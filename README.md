# GiGA Genie 구구단
GiGA Genie 서비스 SDK를 이용한 구구단 게임입니다.

### 사용한 API

Service API는 Callback을 통해 요청 결과를 수신합니다.

 1. `init`으로 API를 초기화


        function init(){
          var options={};
          options.apikey="";
          options.keytype="GBOXDEVM";
          gigagenie.init(options,function(result_cd,result_msg,extra){
            if(result_cd===200){
              status='IS';
              appid=extra.appid;
              console.log('Initialize Success, appid:'+extra.appid);
              startNineNine();
            };
          });
         }

	   * Service SDK 이용을 위해 반드시 초기화를 진행해야 합니다.
	   * options은 다음과 같이 설정합니다.
	     * options.apikey: 개발자 사이트에서 받은 api key (string)
	     * options.keytype: G-Box 개발 키 GBOXDEVM (string)
	   * callback으로 초기화 성공 여부를 수신
	     * result_cd: 200(success)이 아닌 경우 API는 동작하지 않습니다.    


 2. `voice.getVoiceText`으로 음성인식을 요청

        function startNineNine(){
          var options={};
          var numbers=getNumber();
          options.voicemsg=numbers[0]+' '+numbers[1];
          var solution=numbers[0]*numbers[1];
          gigagenie.voice.getVoiceText(options,function(result_cd,result_msg,extra){
            if(result_cd===200){
              console.log(extra.voicetext+':'+solution);
              if(parseInt(extra.voicetext)===solution){
                alert(extra.voicetext+" 정답입니다");
              } else {
                alert(extra.voicetext+" 틀렸습니다.");
              }
            } else {
              alert("다시해보세요");
            }
            startNineNine();
          });
        };

	  * options은 다음과 같이 설정합니다.
	     * options.voicemsg: 음성인식 요청 텍스트 (string), TTS 후 요청 수행
	  * callback으로 음성인식 요청에 대한 결과를 수신합니다.
	     * result_cd가 200(success)인 경우 다음의 값이 전달됩니다.
	     * extra.voicetext: 음성인식 결과 텍스트 (string)


 3. `voice.stopTTS`으로 음성인식을 중단

        function stopTTS() {
          alert("음성인식 중단 요청");
          gigagenie.voice.stopTTS(null,function(result_cd, result_msg, extra) {
            if (result_cd===200) {
              alert("음성인식 중단 성공");
            }
            else if (result_cd===404) {
              alert("음성인식 실행 중이 아님");
            }
            else {
              alert("음성인식 중단 실패: " + result_msg);
            }
          });
        }

	  * options은 null입니다.
	  * callback으로 음성인식 중단 요청에 대한 결과를 수신합니다.


 4. `voice.onRequestClose`으로 서비스 종료 수신

        gigagenie.voice.onRequestClose=function(){
		  gigagenie.voice.svcFinished(null,function(result_cd,result_msg,extra){
		  });
        };

	  * Callback으로 웹 서비스 종료 이벤트를 수신합니다.
	  * 종료 처리를 할 경우 `gigagenie.voice.svcFinished`를 호출해야 합니다.


 5. `media.onMuteRequest`으로 Mute 요청 이벤트 수신

        gigagenie.media.onMuteRequest=function(extra){
          if(extra) console.log("Mute!");
          else console.log("unMute!");
		};

	  * Callback으로 Mute 요청 이벤트를 수신합니다.
	     * extra: 오디오 on/off 값 (boolean)

## License

Copyright 2017 KT corp.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
