# JS_구글 캘린더 API 데이터를 연동한 fullCalenderAPI

1. 구글 클라우드 플랫폼 > 프로젝트 생성

<script src='ko.js'></script>
한글화 옵션을 위한 설정

<script src="https://cdnjs.cloudflare.com/ajax/libs/bPopup/0.11.0/jquery.bpopup.min.js"></script>
팝업생성을 도와주는 jQuery 설정

<스크립트부>

locale: "ko"
달력을 한글로 보이게 지역화

headerToolbar: {
					left: 'prev,next today',
					center: 'title',
					right: 'dayGridMonth,timeGridWeek,timeGridDay,listMonth'
				  }, // 헤더 툴바를 추가 버튼 생성
          
googleCalendarApiKey: 'API 키 입력',

googleCalendarId:'구글 캘린더 ID 입력',

eventClick: function(info) {
					  let start_year = info.event.start.getUTCFullYear(); // info 파라미터를 받아서 날짜 및 시간을 받는다.
					  let start_month = info.event.start.getMonth() + 1; //0부터 시작하니까 +1
					  let start_date = info.event.start.getUTCDate();
					  let start_hour = info.event.start.getHours();
					  let start_minute = info.event.start.getMinutes();
					  let start_second = info.event.start.getSeconds();
					  let end_hour = info.event.end.getHours(); //info 파람으로 받는 일정의 시작지점, 마침시점을 정의

					  let start = start_year + "-" + start_month + "-" + start_date + " " + start_hour + "시 ~ " + end_hour + "시";

					  let attends = "";
					  info.event.extendedProps.attachments.forEach(function(item) {
						  attends += "<div><a href='"+item.fileUrl+"' target='_blank'>[첨부파일]</a></div>"
					  });  //구글 캘린더에서 받아온 정보를 복수형태의 첨부파일을 받아올수있다.

					  if(!info.event.extendedProps.description) {
						  info.event.extendedProps.description = "";
					  } //설명내용이 비어있을때 undefined 상태가 되지 않게끔 공백을 밀어넣게 한다.

					  let contents = `
						<div style='font-weight:bold; font-size:20px; margin-bottom:30px; text-align:center'>
							${start}
						</div>
						<div style='font-size:18px; margin-bottom:20px'>
							제목: ${info.event.title}
						</div>
						<div style='width:500px'>
							${info.event.extendedProps.description}
							${attends}
						</div>
					  `;
            
					 // 일정기간 
					  // 제목
					  // 설명 div 태그를 넣어 contents 변수에 담아둔다.

					  $("#popup").html(contents); //위에서 설정한 contents를 팝업창으로 출력한다.
					  $("#popup").bPopup({ //bPopup을 이용해서 애니매이션 옵션을 지정할수있다.
						speed: 650,
						transition: 'slideIn',
						transitionClose: 'slideBack',
						position: [($(document).width()-500)/2, 30] //x, y
					  });
					  info.jsEvent.stopPropagation(); //원래 작동되야하는 루틴을 막겠다 (일정 클릭시 구글캘린더 사이트로 이동하는 디폴트를 막는다.)
					  info.jsEvent.preventDefault();
				  }
			});
			calendar.render();
		});
