<template>
  <h1 class="h1_style">회원가입</h1>
  <div class="contents">
    <div class="form-wrapper form-wrapper-sm">
      <form @submit.prevent="signup" class="form">
          <div>
              <input  placeholder="이메일" type="text" v-model="credentials.memberId"/>
              <div style="display: inline-block">
              </div> 
              <!-- <div style="display: inline-block"> -->
              <div class="flex-container">
                <p class="duplicateresult">{{ idresult }}</p>
                <h6 type="button" class="duplicatecheck" @click="idchk">중복확인</h6>

              </div>

          </div>
          <div>
              <input class="id_style" placeholder="비밀번호" type="password" v-model="credentials.memberPwd"/>    
          </div>
          <p class="pwlengthcheck" v-if="credentials.memberPwd != '' && (0 < (credentials.memberPwd).length && (credentials.memberPwd).length < 8) ">비밀번호는 최소 8자리 이상 입력해주세요</p>

          <div>
              <input class="pw_style" placeholder="비밀번호확인" type="password" v-model="credentials.passwordConfirmation" @keyup.enter="signup"/>    
          </div>
          <p class="password_alert" v-if=" credentials.passwordConfirmation && (credentials.memberPwd !== credentials.passwordConfirmation)">비밀번호가 일치하지 않습니다.</p>

          <div>
              <input class="name_style" placeholder="닉네임" v-model="credentials.nickName"/>    
          </div>
        <button :disabled="credentials.memberPwd != '' && (0 < (credentials.memberPwd).length && (credentials.memberPwd).length < 8) || idresult == '이미 사용중인 이메일입니다.' || idresult == '이메일 양식에 맞게 입력해주세요'  " class="btn" @click.prevent="signup">회원 가입</button>
      </form>
    </div>
  </div>

</template>

<script>
import { validateEmail } from '@/utils/validation'
import { instance } from '@/api/index.js'

export default {
  name: 'signupForm',
  data() {
      return {
        credentials : {
          memberId: '',
          memberPwd: '',
          passwordConfirmation: '',
          nickName: '',
        },
        idresult: "",
        idformcheck: "",
        pwlengthCheck: ''


      }
    },
  computed: {
    isUseremailValid() {
      return validateEmail(this.credentials.memberId)
    }
  },
  methods: {
    async signup() {
      let msg = "";
      let err = true;

      if (!this.credentials.memberId) {
        msg = "이메일을 입력해주세요";
        err = false;
      } else if (!this.credentials.memberPwd) {
        msg = "비밀번호를 입력해주세요";
        err = false;
      } else if (!this.credentials.passwordConfirmation) {
        msg = "비밀번호확인을 위해 한번 더 입력해주세요";
        err = false;
      } else if (!this.credentials.nickName) {
        msg = "닉네임을 입력해주세요"
        err = false;
      } else if (this.idresult == '') {
        msg = "중복확인을 완료해주세요"
        err = false;
      } else if (this.idresult == '이미 사용중인 이메일입니다.') {
        msg = "중복확인을 완료해주세요"
        err = false;
      } else if (this.credentials.memberPwd != this.credentials.passwordConfirmation) {
        msg = "비밀번호가 일치하지 않습니다";
        err = false
      }
    if (!err) alert(msg);
    else {
      await instance({
        method: "post",
        url: "/api/members/signup/member",  // API 참고
        data: this.credentials,
      }).then((data) => {
        console.log(data);
      });
      this.$router.push('Home');
      alert("이메일 인증 완료 후 로그인해주세요!");
      // this.$router.push('LoginIntroduce');
      
      }
    },
    idchk() {
      if (this.credentials.memberId) {
        instance({
          method: "get",
          url: "/api/members/check/" + this.credentials.memberId,
        }).then((data) => {
          if (data.data == "fail") {  // 반대로 되어있음
            this.idresult = "사용가능한 이메일입니다!";
            if (!this.isUseremailValid) {
              this.idresult = "이메일 양식에 맞게 입력해주세요"
            }
          } else{
            this.idresult = "이미 사용중인 이메일입니다.";
          }
        });
      }
    },
    pwlengthcheck() {
      if (this.credentials.memberPwd != '') {
        if ((this.credentials.memberPwd).length < 8) {
          this.pwlengthCheck = "비밀번호는 8자리 이상 입력해주세요"
        }
      }

    }
    
  }
}    
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;700&display=swap');
.h1_style{
  text-align: center;
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 700;
  padding-top: 5%;
}
.flex-container {
  display: flex;
}
.contents {
  max-width: 1020px;
  margin: 0 auto;
  padding: 0 5px;
  width: 100%;

}
.form-container {
  display: flex;
  align-items: center;
}
.form-wrapper {
  background: white;
  -webkit-box-shadow: 0 20px 20px rgba(0, 0, 0, 0.08);
  box-shadow: 0 20px 20px rgba(0, 0, 0, 0.08);
  border-radius: 3px;
  padding: 15px 15px;
}
.form-wrapper.form-wrapper-sm {
  max-width: 440px;
  margin: 40px auto;
}
.form-wrapper-sm .page-header {
  padding: 0px 0 20px;
}
.form {
	width: 460px;
	height: 100%;
}
.pwlengthcheck {
  color: rgb(247, 9, 21);
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 300;
  font-size: 90%;
}

.form input,
.form textarea {
  font-family: 'Noto Sans KR', sans-serif;
  font-size: 100%;
  width: 89%;
  border: 1px solid #dae1e7;
  box-shadow: 0 2px 4px 0 rgba(0,0,0,.1);
  padding: 0.5rem 0.75rem;
  margin-bottom: 1rem;
  border-radius: 2px 2px 2px 2px;
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 100;

}

.form input:focus {
  outline: 2px solid rgb(75, 137, 220);
}

.btn {
  font-weight: bolder;
  color: white;
  width: 89%;
  box-sizing: border-box;
  background-color: black;
  /* border: 3px solid rgb(75, 137, 220); */
  font-family: 'Noto Sans KR', sans-serif;

}

.btn :hover::before {
  transform: translateY(-100%);
}
.btn:hover {
  font-weight: bolder;
  background-color: rgb(75, 137, 220);
  color: white;
}
.password_alert {
  color: rgb(247, 9, 21);
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 300;
  font-size: 90%;
}
.check {
  display: inline-block;
}
.duplicatecheck {
  font-family: 'Noto Sans KR', sans-serif;
  width: 30%;
  font-weight: 300;
  text-align: right;
  margin-right: 15%;
}
.duplicatecheck:hover {
  font-family: 'Noto Sans KR', sans-serif;
  color: rgb(75, 137, 220);
  text-decoration: underline;
  text-underline-position: under;
  font-weight: bolder;
}
.duplicateresult {
  color: rgb(75, 137, 220);
  font-family: 'Noto Sans KR', sans-serif;
  font-weight: 300;
  font-size: 90%;
  width:160%;
}
</style>