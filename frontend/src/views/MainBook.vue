<template>
  <div class="recommend-back">

    <!-- 페이지 로딩 -->
    <div v-show="isLoading" id="page-loading">
      <div class="three-balls">
        <div class="ball ball1"></div>
        <div class="ball ball2"></div>
        <div class="ball ball3"></div>
      </div>
    </div>

    <!-- 로딩 후 책장 -->
    <div v-show="!isLoading" class="shelf">
      <div 
        id="first-shelf-tag"
        class="tag"
      >
        <div 
          class="tag-name"
        >
          <span v-if="loginState" id="first-tag">
            맞춤형 추천
          </span>
          <div v-else>
            <span id="first-tag">
              실시간 도서
            </span>
            <span class="no-login">맞춤형 추천은 <span @click="goToLogin" class="login-letter hvr-grow">로그인</span>시에만 가능합니다.</span>
          </div>

        </div>
      </div>
      <div 
        id="first-row"
        class="shelf-row"
      >
        <div class="d-flex justify-content-around books" style="margin-right: 40px;">
          <!-- 클릭할때 객체를 스토어에 저장 commit -->
          <Book 
            v-for="(book, idx) in suitRecommend" 
            :key="idx" 
            class="book-component mouse-pointer" 
            :book="book" 
            @click="selectBook(book)"
            @open-modal="openModal"
            @delete-readBook="deleteReadBook"
          ></Book>
        </div>
        <!-- <Book-shelf /> -->
      </div>
      <div
        id="second-shelf-tag"
        class="tag"
      >
        <div 
          class="tag-name"
        >
          <span v-if="loginState" id="second-tag">
            {{ userGenre }}
          </span>
          <span v-else style="height: 51px;"></span>
        </div>
      </div>
      <div 
        id="second-row"
        class="shelf-row"
      >
        <div class="d-flex justify-content-around books" style="margin-left: 40px;">
          <Book 
            v-for="(book, idx) in genreRecommend" 
            :key="idx" 
            class="book-component mouse-pointer" 
            :book="book" 
            @click="selectBook(book)"
            @open-modal="openModal"
            @delete-readBook="deleteReadBook"
          ></Book>
        </div>
        <!-- <Book-shelf /> -->
      </div>
      <div
        id="third-shelf-tag"
        class="tag"
      >
        <div 
          class="tag-name"
        >
          <span v-if="loginState" id="third-tag">
            위시리스트
          </span>
          <span v-else style="height: 51px;"></span>
        </div>
      </div>
      <div
        id="third-row"
        class="shelf-row"
      >
        <div class="d-flex justify-content-around books" style="margin-right: 40px;">
          <Book 
            v-for="(book, idx) in wishRecommend" 
            :key="idx" 
            class="book-component mouse-pointer" 
            :book="book" 
            @click="selectBook(book)"
            @open-modal="openModal"
            @delete-readBook="deleteReadBook"
          ></Book>
        </div>
        <!-- <Book-shelf /> -->
      </div>
      <Modal v-show="isModalViewed" @close-modal="closeModal">
          <!-- 컨텐츠 컴포넌트 자리 -->
          <!-- 헤더 자리 -->
          <template #header>

          </template>

          <!-- 바디 자리 -->
          <template #body>
            <CollectSentence v-if="step === 'collectSentence' " :book="selectedBook" :mode="0" @close-modal="closeModal"/>
          </template>
      </Modal> 
    </div>
  </div>
</template>

<script>
import Book from '@/components/Main/Book.vue'
import Modal from '@/components/Element/Modal.vue'
import CollectSentence from '@/components/Book/CollectSentence.vue'

export default {
  name: 'Main',
  components: {
    Book,
    Modal,
    CollectSentence,
  },
  data() {
    return {
      suitRecommend: [],
      genreRecommend: [],
      userGenre: "",
      wishRecommend: [],
      isModalViewed: false,
      selectedBook: "",
      step: "",
      isLoading: true,
      loginState: false,
    }
  },
  created() {
    if (localStorage.getItem('jwt')) {
      this.loginState = true
    } else {
      this.loginState = false
    }
  },
  mounted() {
    this.loadBookData()
  },
  methods: {
    // 책을 로드하는 함수
    loadBookData() {
      let bucket = new Set()
      while (bucket.size < 12) {
        let num = Math.floor(Math.random() * (19))
        bucket.add(num)
      }
      const array = [...bucket]
      // 플라스크 서버 처리 이후 수정
      const RecommendList = this.$store.getters.getRecommendList
      let i = 0;

      if (RecommendList) {
        // 응답값 확인 후 랜덤 메서드 넣기 -----------------------------------------------^_^
        this.suitRecommend = []
        this.genreRecommend = []
        this.wishRecommend = []
        for (i = 0; i < 4; i++) {
          this.suitRecommend.push(RecommendList.customized_by_user[array[i]])
          this.genreRecommend.push(RecommendList.customized_by_genre.customized_books[array[i + 4]])
          if (RecommendList.customized_by_wishlist.length) {
            this.wishRecommend.push(RecommendList.customized_by_wishlist[array[i + 8]])
          }
        }
            
        this.userGenre = RecommendList.customized_by_genre.genre.genre_name
        setTimeout(() => {
          this.isLoading = false
        }, 500);
      } else {
        // 없다면 책정보 요청하기
        const token = localStorage.getItem('jwt')
        const headers = {
          "Authorization": token
        }
        if (token) {
          this.$axios.get(`${this.$store.getters.getServer}/recommend`, {headers})
          .then(res => {
            // 랜덤 값 추출 추가해야함 ------------------------------------------------------
            for (i = 0; i < 4; i++) {
              this.suitRecommend.push(res.data.data.customized_by_user[array[i]])
              this.genreRecommend.push(res.data.data.customized_by_genre.customized_books[array[i + 4]])
              if (res.data.data.customized_by_wishlist.length) {
                this.wishRecommend.push(res.data.data.customized_by_wishlist[array[i + 8]])
              }
            }
            this.userGenre = res.data.data.customized_by_genre.genre.genre_name
            // 받아온 책을 store에 저장
            this.$store.commit('SaveRecommendList', res.data.data)
            this.isLoading = false
          })
          .catch(err => {
            console.error(err)
          })
        } else {
          this.$axios.get(`${this.$store.getters.getServer}/recommend`)
          .then(res => {
            // 랜덤 값 추출 추가해야함 ------------------------------------------------------
            for (i = 0; i < 4; i++) {
              this.suitRecommend.push(res.data.data.customized_by_user[array[i]])
              this.genreRecommend.push(res.data.data.customized_by_genre.customized_books[array[i + 4]])
              if (res.data.data.customized_by_wishlist.length) {
                this.wishRecommend.push(res.data.data.customized_by_wishlist[array[i + 8]])
              }
            }
            this.userGenre = res.data.data.customized_by_genre.genre.genre_name
            this.isLoading = false
          })
          .catch(err => {
            console.error(err)
          })
        }
      }
    },
    closeModal() {
      this.isModalViewed = false
      // console.log('닫아')
    },
    openModal(book) {
      // 선택한객체를 변수와 store에 모두 저장
      // 변수는 책 선택시 바로 커버를 띄울 목적
      this.selectedBook = book
      // store는 반응이 늦으므로 이후 axios요청을 보낼 목적
      this.selectBook(book)
      this.step = 'collectSentence'
      this.isModalViewed = true
      // console.log('열어')
    },
    // deleteReadBook(isbn) {
    deleteReadBook() {
      // console.log(isbn)
      // 읽은 책 목록 삭제하기
      // 1. 위시리스트 기반 추천이 완료되면 data 분리(현재 유저기반 공유중)
      // 2. 각 추천별 id 부여 => props로 전달
      // 3. 삭제요청 보낼때 props가 가진 번호별로 data 를 선택하고 isbn과 일치여부 판별
      // 4. 해당 data리스트에서 책 삭제하기
    },
    goToReaction() {
      // console.log('되나?')
      this.step = "bookReaction"
    },
    selectBook(book) {
      this.$store.commit('SelectBook', book)
    },
    getSelectedBook() {
      this.selectedBook = this.$store.getters.getSelectedBook
    },
    goToCollect() {
      this.step = "collectSentence"
    },
    goToLogin() {
      this.$router.push({ name: "Login" })
    }
  }
}
</script>

<style>
  .no-login {
    font-size: 1rem;
    font-weight: normal;
  }
  .login-letter {
    font-weight: bold;
  }
  .login-letter:hover {
    cursor: pointer;
    color: #006ea1;
  }
  .recommend-back {
    /* background-image: url('../assets/waterprint_back.jpg'); */
    background-repeat: no-repeat;
    background-size: 100%;
    padding: 0px;
    min-width: 1140px;
  }
  .shelf{
    margin-top: 40px;
    background-color: rgba(237, 234, 232, 0.7);
    border-radius: 10px;
  }
  .shelf-row{
    text-align: center; margin: 0 auto;
  }
  .books {
    width: 950px;
    margin: auto;
  }
  .tag{
    text-align: left;
    margin-top: -20px;
    margin-left: 50px;
  }
  .tag-name{
    margin-bottom: 20px;
    margin-top: 1.5rem;
    /* background-color: rgba(161, 114, 70, 0); */
    display: inline-block;
    font-size: 1.7rem;
    font-weight: bold;
    padding: 10px;
    margin-left: 0px;
    /* box-shadow: 1px 1px 2px rgb(150, 150, 150); */
    border-radius: 15px/ 15px;
    border: 0px;
    color: black;
    /* color: rgb(0, 0, 0); */
    align-items: left;
    text-align: left;
    width: 1000px;
    /* text-shadow: white 2px 3px; */
    /* background-color: rgba(255, 255, 255, 0.3); */
  }
  .tag-name > span {
    /* background-color: rgba(0, 0, 0, 0.8); */
    padding: 5px 15px 5px;
    border-bottom: grey dotted 1px !important;
  }
  .tag-name > span[id="first-tag"] {
    /* margin-right: 100px; */
    border-bottom: #FB6542 solid 1px;
    display: flex;
  }
  .tag-name > span[id="second-tag"] {
    border-bottom: #03A9F4 solid 1px;
    /* margin-left: 100px; */
    display: flex;
    justify-content: flex-end;
  }
  .tag-name > span[id="third-tag"] {
    /* margin-right: 100px; */
    border-bottom: #FFBB00 solid 1px;
    display: flex;
  }

  /* 페이지 로딩 css */
  #page-loading {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 999;
  background-color: rgba(237, 234, 232, 0.7);
}

.three-balls {
  margin: 0 auto;
  width: 120px;
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 45%;
}

.three-balls .ball {
  position: relative;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: inline-block;
  -webkit-animation: bouncedelay 2.0s infinite cubic-bezier(.62, .28, .23, .99) both;
  animation: bouncedelay 2.0s infinite cubic-bezier(.62, .28, .23, .99) both;
}

.three-balls .ball1 {
  -webkit-animation-delay: -.24s;
  animation-delay: -.24s;
}

.three-balls .ball2 {
  -webkit-animation-delay: -.12s;
  animation-delay: -.12s;
  margin-left: 10px;
  margin-right: 10px;
}

@keyframes bouncedelay {
  0% {
    bottom: 0;
    background-color: #03A9F4;
  }
  16.66% {
    bottom: 40px;
    background-color: #FB6542;
  }
  33.33% {
    bottom: 0px;
    background-color: #FB6542;
  }
  50% {
    bottom: 40px;
    background-color: #FFBB00;
  }
  66.66% {
    bottom: 0px;
    background-color: #FFBB00;
  }
  83.33% {
    bottom: 40px;
    background-color: #03A9F4;
  }
  100% {
    bottom: 0;
    background-color: #03A9F4;
  }
}
</style>