---
layout: post
title: '자바스크립트 정규표현식을 활용한 주민등록번호,전화번호,숫자 체크'
author : 효준
date: 2018-07-24 10:03
tags: [javascript]
image: /files/covers/blog.jpg
---

# 자바스크립트 정규표현식을 이용하여 주민등록번호 6자리 이후 - 자동으로 생성


        var jumin = obj;
        var str1 = jumin.val().substring(0,6);
        var str2 = jumin.val().substring(6,jumin.val().length);

        if(jumin.val().length>6) {
            if(jumin.val().length>=13) {
                str2 = jumin.val().substring(6,13);
                $(jumin).val(str1+'-'+str2);
            }else {
                $(jumin).val(str1+'-'+str2);
            }
        }else {
            str1 = jumin.val().substring(0,jumin.val().length);
            $(jumin).val(str1);
        }

이 코드는 자동으로 숫자 6개이후 -가 붙도록 하드코딩한 상태이고 리팩토링이 필요한 코드이긴 하다.
하지만 작동은 한다.
keydown이벤트가 아닌 keyup이벤트를 input에 걸어야한다.

    $("#jumin").off().on('keyup',function (e) {

                if(e.keyCode==8||e.keyCode==39||e.keyCode==37){
                    return false;
                }

                $("#jumin").val($("#jumin").val().replace(/[^0-9]/g,""));
                jumin.checkNum($("#jumin"), e);

            });

id가 jumin인 인풋에 key up 이벤트를 걸었으며 키업이벤트가 일어날때마다 정규식으로 체크하여 숫자가 아닌글자들은
지워지게 된다.

            var value = $("#jumin").val();
            var reg = /^(?:[0-9]{2}(?:0[1-9]|1[0-2])(?:0[1-9]|[1,2][0-9]|3[0,1]))-[1-4][0-9]{6}$/;

            if(!reg.test(value)) {
                $("#checkTxt").show();
            } else {
                alert("정상입니다.");
                $("#checkTxt").hide();
            }

이 코드는 주민등록번호를 완성했을때 정상적인 주민등록번호인지 확인하는 것이다.

# 숫자를 금액처럼 , 자동추가하기

     addComma : function (num) { // 들어온 매개변수에 ,를 삽입해서 리턴
             var regexp = /\B(?=(\d{3})+(?!\d))/g;
             return num.toString().replace(regexp, ',');
         }

 이 함수는 들어온 매개변수에 3자리마다 ,를 자동 추가해주는 함수이다.

    checkNm : function () {
            $("#money").val($("#money").val().replace(/[^0-9]/g,""));
            var num = $("#money").val();
            $("#money").val(money.addComma(num));

        }
역시 keyup이벤트가 발생할때마다 숫자가 아닌것은 지워주고 마지막줄에 val 매개변수로 money.addComma(num)
지정해주면 자동으로 keyup이 일어날때마다 그 인풋박스에 숫자에 자동으로 콤마(,)를 붙여준값이 셋팅된다.

# 전화번호에 자동으로 - 붙여주기

    var phoneClass = {
        flag : false,
        init : function () {
            phoneClass.bind();
        },
        bind : function () {
            $("#phone").off().on('keyup',function (e) {
                if(e.keyCode==8||e.keyCode==39||e.keyCode==37){
                    return false;
                }

                $("#phone").val($("#phone").val().replace(/[^0-9]/g,""));
                phoneClass.flag = false;
                phoneClass.checkPhone(e);
            });
        },
        checkPhone : function (e) {
            var phone = $("#phone");
            var phoneArr = ['010','011','016','018','012','015','019','017'];
            var homeArr=['02','031','032','033','041','042','043','044','051','052','053','054','055','061','062','063','064'];




            if(phone.val().length > 2) {
                var head = phone.val().substring(0,2);
                var body = phone.val().substring(2,phone.val().length);
                var i;


                if(head == '02') {  // 지역번호2개짜리는 서울밖에없음 .
                    if(body.length >= 3) {
                        phone.val(head+'-'+body+'-');
                        if(body.length <= 7) {
                            var tail = body.substring(3,phone.val().length);
                            body = body.substring(0,3);
                            phone.val(head+'-'+body+'-'+tail);
                        }else if(body.length == 8) {
                            var tail = body.substring(4,8);
                            body = body.substring(0,4);
                            phone.val(head+'-'+body+'-'+tail);
                        }else if(body.length == 9) {
                            var tail = body.substring(4,8);
                            body = body.substring(0,4);
                            phone.val(head+'-'+body+'-'+tail);
                            alert("전화번호가 이상합니다.");
                            return false;
                        }
                    }else {
                        phone.val(head+'-'+body);
                    }
                } else {
                    head = phone.val().substring(0,3);
                    body = phone.val().substring(3,phone.val().length);

                    for(i = 0 ; i < phoneArr.length ; i++) {
                        if(head == phoneArr[i]) {
                            phoneClass.flag = true;
                            break;
                        }
                    }
                    if(phoneClass.flag == false) {
                        for(i = 0 ; i < homeArr.length ; i++) {
                            if(head == homeArr[i]) {
                                phoneClass.flag = true;
                                break;
                            }
                        }
                    }

                    if(phoneClass.flag == false) {
                        alert("지역번호,폰시작번호가 이상합니다.");
                        $("#phone").val("");
                        return false;
                    }

                    if(body.length >= 3) {
                        phone.val(head+'-'+body+'-');
                        if(body.length <= 7) {
                            var tail = body.substring(3,phone.val().length);
                            body = body.substring(0,3);
                            phone.val(head+'-'+body+'-'+tail);
                        }else if(body.length == 8) {
                            var tail = body.substring(4,8);
                            body = body.substring(0,4);
                            phone.val(head+'-'+body+'-'+tail);
                        }else if(body.length == 9) {
                            var tail = body.substring(4,8);
                            body = body.substring(0,4);
                            phone.val(head+'-'+body+'-'+tail);
                            alert("전화번호가 이상합니다.");
                            return false;
                        }
                    }else {
                        phone.val(head+'-'+body);
                    }
                }
            }
        }
    }

이 코드 또한 하드코딩한 부분인데 인풋에 숫자를 적으면 맨처음 지역번호가 이상하면 다 지워준다.
또한 xxx-xxx-xxxx , xx-xxxx-xxxx 등 많은타입의 숫자들이있는데 만약 지역번호를 더 추가하고싶다면 배열요소에
추가하면된다.
