<template>
  <div style="text-align: center; margin: 0 auto;">
    <img
      width=120px;
      height=120px; 
      src="data:image/svg+xml;base64,PHN2ZyBpZD0iZmxhdCIgaGVpZ2h0PSI1MTIiIHZpZXdCb3g9IjAgMCA1MTIgNTEyIiB3aWR0aD0iNTEyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxwYXRoIGQ9Im0zNjAgMzY4LTE2IDE2LTExLjE2IDMuODItNzUuMyAyNS43Ni03NS42OSAyNS44OS03NC45MSAyNS42My02Ni45NCAyMi45aC0xNnYtMTZsMTMuNjktMzIuODYgMTkuNDYtNDYuNzEgMTkuNDYtNDYuNyAyMC44OS01MC4xMyAxOC4wMy00My4yNyAxOS40Ni00Ni43IDkuMDEtMjEuNjMgMTYtMTYgNzUuMiA3NS4yIDg2LjM5IDg2LjM5eiIgZmlsbD0iI2Q2OTY1MSIvPjxnIGZpbGw9IiNjNzg2NDAiPjxwYXRoIGQ9Im0xODEuODUgNDM5LjQ3LTc0LjkxIDI1LjYzLTY5LjI1LTI1Ljk2IDE5LjQ2LTQ2LjcxeiIvPjxwYXRoIGQ9Im0zMzIuODQgMzg3LjgyLTc1LjMgMjUuNzYtMTgwLjkzLTY3Ljg1IDIwLjg5LTUwLjEzeiIvPjxwYXRoIGQ9Im0zMjEuNTkgMzI5LjU5LTIwNi4wNi03Ny4yNiAxOS40Ni00Ni43IDEwMC4yMSAzNy41N3oiLz48L2c+PHBhdGggZD0ibTIwMCAxNjAgMzMuNDUyNTMtMzMuNDUyNTNhMjIuMzU0NDcgMjIuMzU0NDcgMCAwIDAgNi41NDc0Ny0xNS44MDY5OSAyMi4zNTQ0OSAyMi4zNTQ0OSAwIDAgMCAtMTMuNTQ4NjEtMjAuNTQ3bC0yMC44MzAxOS04LjkyNzI1YTg4IDg4IDAgMCAxIC0zNS43MzUxLTI4LjA4NDc2bC05Ljg4NjEtMTMuMTgxNDcgMjQtMTZhODQuMDY4IDg0LjA2OCAwIDAgMCA0My4zNjQxNiAzMy4xMjEzN2w5LjUyODI4IDMuMTc2MWE1MS4zNDM5MSA1MS4zNDM5MSAwIDAgMSAzNS4xMDc1NiA0OC43MDkxMyA1MS4zNDM4OCA1MS4zNDM4OCAwIDAgMSAtMTIuMzYwNzEgMzMuNDE0MTdsLTM1LjYzOTI5IDQxLjU3OTIzeiIgZmlsbD0iI2ZmYzQzMSIvPjxwYXRoIGQ9Im0zMTIgMTIwLTgtNDggNDggMTZ6IiBmaWxsPSIjZDFlNGZmIi8+PHBhdGggZD0ibTI3MiAxOS41MDhoMTZ2MzIuOTg1aC0xNnoiIGZpbGw9IiNmYjZhODMiIHRyYW5zZm9ybT0ibWF0cml4KC4yNDMgLS45NyAuOTcgLjI0MyAxNzcuMTY1IDI5OC45MDkpIi8+PHBhdGggZD0ibTM0MS41NzggMjRoMjguODQ0djE2aC0yOC44NDR6IiBmaWxsPSIjZmZjNDMxIiB0cmFuc2Zvcm09Im1hdHJpeCguODMyIC0uNTU1IC41NTUgLjgzMiA0Mi4wNCAyMDIuODQ4KSIvPjxwYXRoIGQ9Im0xNTYgODcuNTA4aDE2djMyLjk4NWgtMTZ6IiBmaWxsPSIjNjE3M2U4IiB0cmFuc2Zvcm09Im1hdHJpeCguOTcgLS4yNDMgLjI0MyAuOTcgLTIwLjMyNyA0Mi44ODEpIi8+PHBhdGggZD0ibTM1MiAzMTIgMzMuNDUyNTMtMzMuNDUyNTNhMjIuMzU0NDcgMjIuMzU0NDcgMCAwIDEgMTUuODA2OTktNi41NDc0NyAyMi4zNTQ0OSAyMi4zNTQ0OSAwIDAgMSAyMC41NDcgMTMuNTQ4NjFsOC45MjcyMiAyMC44MzAxOWE4OCA4OCAwIDAgMCAyOC4wODQ3NiAzNS43MzUxbDEzLjE4MTUgOS44ODYxIDE2LTI0YTg0LjA2OCA4NC4wNjggMCAwIDEgLTMzLjEyMTM3LTQzLjM2NDE2bC0zLjE3NjEtOS41MjgyOGE1MS4zNDM5MSA1MS4zNDM5MSAwIDAgMCAtNDguNzA5MTMtMzUuMTA3NTYgNTEuMzQzODggNTEuMzQzODggMCAwIDAgLTMzLjQxNDE3IDEyLjM2MDcxbC00MS41NzkyMyAzNS42MzkyOXoiIGZpbGw9IiM2MTczZTgiLz48cGF0aCBkPSJtNDE2IDIwOCA0OCA4LTE2LTQ4eiIgZmlsbD0iI2ZmYzQzMSIvPjxwYXRoIGQ9Im00NjggMjM5LjUwOGgxNnYzMi45ODVoLTE2eiIgZmlsbD0iI2ZiNmE4MyIgdHJhbnNmb3JtPSJtYXRyaXgoLjk3IC0uMjQzIC4yNDMgLjk3IC00Ny44NzcgMTIzLjA5KSIvPjxwYXRoIGQ9Im00NjUuNTc4IDE0OGgyOC44NDR2MTZoLTI4Ljg0NHoiIGZpbGw9IiNkMWU0ZmYiIHRyYW5zZm9ybT0ibWF0cml4KC41NTUgLS44MzIgLjgzMiAuNTU1IDgzLjk0NCA0NjguODUxKSIvPjxwYXRoIGQ9Im00MDAgMzMxLjUwOGgxNnYzMi45ODVoLTE2eiIgZmlsbD0iIzYxNzNlOCIgdHJhbnNmb3JtPSJtYXRyaXgoLjI0MyAtLjk3IC45NyAuMjQzIC0yOC41NjQgNjU5LjQxNikiLz48cGF0aCBkPSJtMjU2IDIyNCAzMS44NjE0Ny0zNy4xOTU5NGE2Mi43MDQzOCA2Mi43MDQzOCAwIDAgMSA0MC43MDEtMjEuNTMzMjJsLjkwODM2LS4xMDA5M2E2NS4zMzYwOSA2NS4zMzYwOSAwIDAgMCA1Ni42NTY2NS01MC43NDM0di0uMDAwMDdhNDQuODYyIDQ0Ljg2MiAwIDAgMSAzMS45ODk3Mi0zMy41NDkzMWw2MS44ODI4LTE2Ljg3NzEzIDggMzItNDMuMjYwNTkgOS42MTM0NmE0NS4wMjk2NCA0NS4wMjk2NCAwIDAgMCAtMzQuNDE2MjcgMzUuMjc0NTRsLS4yMjcxNCAxLjE1NTg1YTY4LjU1NjkxIDY4LjU1NjkxIDAgMCAxIC02MC40NDgxMyA1NC45OTEzOWwtLjk0NTQ2LjA5NDU1YTcwLjE4MiA3MC4xODIgMCAwIDAgLTQ3LjExMTA5IDI0LjkzNTM0Yy0xMC40ODUwNCAxMi42MDczNC0yMS41OTEzMiAyNS45MzQ4Ny0yMS41OTEzMiAyNS45MzQ4N3oiIGZpbGw9IiNmYjZhODMiLz48L3N2Zz4=" 
    />
    <h3
      class="complete-title"
    >
      회원가입이 완료되었습니다!</h3>
    <p class="notice-message notice-message-margin">
      조금 더 정확한 추천을 위해 선택항목을 입력해주세요.
    </p>
    <div class="button-group">
      <router-link 
        :to="{ name: 'SubmitGender'}" 
        class="btn btn-success complete-button"
      >
        <span class="button-text">
          네, 좋아요😊
        </span>
      </router-link>
      <router-link 
        :to="{ name: 'MainBook' }" 
        class="btn btn-danger complete-button"
      >
        귀찮아요
        <br>
        다음에 할게요
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pageData: {
        progress: 4,
        size: "back-md"
      },
    }
  },
  created() {
    this.signupComplete()
    // console.log(this.$store.getters.getUser)
    const signupInfo = this.$store.getters.getSignupInfo
    // console.log(signupInfo, 'signupInfo')
    this.$axios.post(`${this.$store.getters.getServer}/user/join`, signupInfo)
    .then(() => {
      // console.log('회원가입 결과!')
      // console.log(res)
      this.login()
    })
    .catch(err => {
      console.error(err)
    })
  },
  methods: {
    signupComplete() {
      this.$emit('nowPage', "signupComplete")
    },
    servePageInfo() {
      // console.log("여기는 회원가입완료")
      return this.pageData
    },
    login() {
      const user = {
        "userEmail": this.$store.getters.getSignupInfo.userEmail,
        "userPassword": this.$store.getters.getSignupInfo.userPassword,
        "mode": 0
      }
      this.$store.dispatch('Login', user)
    },
  }
}
</script>

<style>
  .notice-message-margin {
    margin: 8px 0 20px 0;
  }
  .complete-title {
    justify-content: center;
    font-weight: bold;
    margin-top: 1.5rem;
  }
  .button-group {
    display: flex;
    justify-content: space-around;
    margin-top: 2rem;
  }
  .complete-button {
    border-radius: 15px / 15px;
  }
  .button-text {
    display: inline-block; 
    margin: 15px 0;
  }
</style>