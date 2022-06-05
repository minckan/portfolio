<template>
  <div id="main">
    <div class="invisible_background" @click="onClickAnywhere()"/>
    <section class="header">
      <!-- full header -->
      <div class="full_header">
        <div class="background-wrapper">
          <lottie :options="defaultOptions" @animCreated="handleAnimation"/>
        </div>

        <div class="faq_button" @click="onMoveToFaq">FAQ</div>
        <div class="center_text_wrapper">
          <img class="logo" src="@/assets/image/logo_w.png" alt="로고">
          <p class="headline">
            액션 차트는 음원 판매량과 관계 없이<br>현재 가장 이슈가 있는 음원을 평가하는 차트입니다.
          </p>
          <dl>
            <dt><img src="@/assets/image/icon_youtube.png" alt=""></dt>
            <dt><img src="@/assets/image/icon_google.png" alt=""></dt>
            <dt><img src="@/assets/image/icon_naver.png" alt=""></dt>
            <dt><img src="@/assets/image/icon_facebook.png" alt=""></dt>
            <dt><img src="@/assets/image/icon_twitter.png" alt=""></dt>
          </dl>
          <p class="subline">음원의 온라인 이슈를 영상 채널, 검색엔진, SNS 등에서 탐지하여<br>지표의 변화량, 채널의 영향력, 생산성과 반응성 등을 종합적으로 평가합니다.</p>
        </div>
      </div>

      <!-- small-header -->
      <div-half-header class="half_header">
        <div class="inner">
          <div class="logo_wrapper"><img src="@/assets/image/logo_bk.png" alt=""></div>
          <div class="faq_button" @click="onMoveToFaq">FAQ</div>
        </div>
      </div-half-header>

    </section>

    <section class="body">
      <div class="body-inner-wrap">

        <div class="body_header">
          <dl class="tab_group">
            <dt :class="isDayList ? 'on left_tab' : 'left_tab'" @click="isDayList = true">일간</dt>
            <dt :class="!isDayList ? 'on right_tab' : 'right_tab'" @click="isDayList = false">주간</dt>
          </dl>

          <dl class="date_picker">
            <dt class="leftBtn" @click="onChangeToPrev()"/>
            <dt class="centerDt" @click="showCalendarView = true">
              <p v-if="isDayList">{{ date }}</p>
              <p v-else>{{ week }}</p>
              <div class="cal_img"/>
            </dt>
            <dt class="rightBtn" @click="onChangeToNext()"/>
          </dl>

          <div v-if="showCalendarView" class="calendar_view">
            <dl class="cal_header">
              <div class="today_sel_btn" @click="openDate = $moment()">오늘</div>
              <div class="close_btn"><img src="@/assets/image/close.png" alt="" @click="showCalendarView = false"></div>
              <dt class="leftBtn" @click="onClickPrevBtn()"/>
              <dt class="centerDt">
                <p>{{ month }}</p>
                <div class="cal_img"/>
              </dt>
              <dt class="rightBtn" @click="onClickNextBtn()"/>
            </dl>
            <dl class="cal_body">
              <dt class="cal">
                <datepicker
                  v-model="select"
                  :language="language"
                  :inline="true"
                  :highlighted="highlightedFn"
                  :open-date="openDate"
                  :day-cell-content="dayCellContent"
                  :minimum-view="'day'"
                  :maximum-view="'day'"
                  :disabled-dates="disabledDates"
                  @selected="dateSelected"/>
              </dt>
              <dt class="week_sel">
                <p class="week_header">주간</p>
                <p v-for="( item , w) in weeks" :key="w" class="week_body" :class="item.checked ? 'on' : !item.isValid ? 'off' : ''" @click="weekSelected(w, item.isValid)">{{ item.weekNm }}</p>
              </dt>
            </dl>
          </div>

        </div>
        <div class="body_contents_wrapper">
          <dl class="column_wrapper">
            <dt class="">순위</dt>
            <dt class="">곡아티스트</dt>
            <dt class="">앨범</dt>
            <dt class="">액션지수</dt>
          </dl>
          <dl v-for="(list, l) in albumList" :key="l" class="list-wrapper">
            <dt class="rank">
              <h1>{{ list.rank }}</h1>
              <div class="upanddown_wrapper">
                <template v-if="list.status === 'UP'">
                  <p style="color: #DE6153;">{{ list.changed }}</p>
                  <img src="@/assets/image/up.png" alt="">
                </template>
                <template v-else-if="list.status === 'DOWN'">
                  <p class="blue-text">{{ list.changed }}</p>
                  <img src="@/assets/image/down.png" alt="">
                </template>
                <template v-else-if="list.status === 'NEW'"><p style="color: #DE6153; font-family: Roboto-Italic;">NEW</p></template>
                <template v-else-if="list.status === 'RE_ENTRY'"><p style="color: #7565E9; font-family: Roboto-Italic;">RE<br>ENTRY</p></template>
                <template v-else>
                  <p class="gray-text">-</p>
                </template>
              </div>
            </dt>
            <dt class="artist_info" @click="onShowMv(l)">
              <div class="img_wrap"><img :src="list.thumbnailImg" alt=""></div>
              <div class="right_name_wrap">
                <div class="inner">
                  <p class="title">{{ list.title }}</p>
                  <p class="artist">{{ list.artist }}</p>
                </div>
              </div>
            </dt>
            <dt class="album_info">
              <p>{{ list.albumNm }}</p>
            </dt>
            <dt class="action_score">
              <div class="inner">
                <h1>{{ list.actionScore }}</h1>
                <div class="upanddown_wrapper">
                  <p>
                    <span v-if="list.actionChanged !== '0'" :class="list.actionChanged | checkStat">{{ list.actionChanged }}</span>
                    <span v-else :class="gray-text">-</span>
                  </p>
                </div>
              </div>
            </dt>
            <div v-show="list.showMv" class="movie_popup">
              <div class="close_btn" @click="list.showMv = false"><img src="@/assets/image/close_bk.png" alt=""></div>
              <dl class="popup_header_area">
                <div class="prev_btn" @click="onPrevMovie(l)"><img src="@/assets/image/prev_btn.png" alt=""></div>
                <div class="title_wrapper">
                  <p class="title">{{ list.title }}</p>
                  <p class="artist">{{ list.artist }}</p>
                </div>
                <div class="next_btn" @click="onNextMovie(l)"><img src="@/assets/image/next_btn.png" alt=""></div>
              </dl>
              <div class="popup_content_area">
                <!-- 유튜브 삽입 부분 -->
                <img src="@/assets/image/temp/movie.png" alt="">
                <!-- 실사용에서는 데이터 바인딩
                    <img :src="list.movie" alt=""> -->
              </div>
            </div>
          </dl>
        </div>
      </div>
    </section>

    <section class="footer">
      <dl class="faq_wrapper">
        <dt class="faq_left">
          <h3 class="title">고객센터</h3>
          <h1 class="cs_number">1544-5597</h1>
          <h4 class="operating_time">AM 10:00 ~ PM 05:00 KST<br>(토/일/공휴일 휴무)</h4>
          <div class="req_btn"><p>문의하기</p></div>
        </dt>
        <dt class="faq_list_wrapper">
          <p class="faq_text">FAQ</p>
          <div class="faq_right">
            <div v-for="(faq, f) in faqList" :key="f" class="question_wrapper" @click="faq.show = !faq.show">
              <div class="question"><p>{{ faq.q }}</p></div>
              <div v-show="faq.show" class="answer"><p v-html="faq.a"/></div>
            </div>
          </div>
        </dt>
      </dl>
      <div class="very_last_footer">
        <p>ⓒ12cm</p>
      </div>
    </section>

    <div v-if="isScrollDown" class="move-top-btn" @click="onMoveToTop()">
      <p>TOP</p>
    </div>

  </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker/dist/vuejs-datepicker.esm.js'
import ko from 'vuejs-datepicker/src/locale/translations/ko'
import Lottie from 'vue-lottie'
import animationData from '@/assets/lottie/motion.json'

export default {
  name: 'Main',
  components: {
    Datepicker, Lottie
  },
  filters: {
    checkStat: function(value) {
      const substr = value.substr(0, 1)
      return substr === '+' ? 'red-text' : 'blue-text'
    }
  },
  data() {
    return {
      defaultOptions: { animationData: animationData },
      animationSpeed: 1,
      delay: false,
      language: ko,
      select: null,
      showCalendarView: false,
      albumList: [
        {
          rank: 1,
          status: 'N/C',
          rankChanged: 0,
          thumbnailImg: require('@/assets/image/temp/1.png'),
          title: '밤하늘의 별을 (2020)',
          artist: '경서',
          albumNm: '밤하늘의 별을 (2020)',
          actionScore: 9999,
          actionChanged: '0',
          showMv: false
        },
        {
          rank: 2,
          status: 'UP',
          changed: 2,
          thumbnailImg: require('@/assets/image/temp/2.png'),
          title: 'Dynamite',
          artist: '방탄소년단',
          albumNm: 'Dynamite (DayTime Version)',
          actionScore: 8500,
          actionChanged: '0',
          showMv: false
        },
        {
          rank: 3,
          status: 'UP',
          changed: 1,
          thumbnailImg: require('@/assets/image/temp/3.png'),
          title: 'VVS (Feat. JUSTHIS)(Prod. GroovyRoom)',
          artist: '미란이 (Mirani)',
          albumNm: '쇼미더머니 9 Episode 1',
          actionScore: 7652,
          actionChanged: '+86',
          showMv: false
        },
        {
          rank: 4,
          status: 'DOWN',
          changed: 1,
          thumbnailImg: require('@/assets/image/temp/4.png'),
          title: '힘든 건 사랑이 아니다',
          artist: '임창정',
          albumNm: '힘든 건 사랑이 아니다',
          actionScore: 6521,
          actionChanged: '-2',
          showMv: false
        },
        {
          rank: 5,
          status: 'UP',
          changed: 495,
          thumbnailImg: require('@/assets/image/temp/5.png'),
          title: '딩가딩가 (Dingga)',
          artist: '마마무 (Mamamoo)',
          albumNm: '딩가딩가 (Dingga)',
          actionScore: 4521,
          actionChanged: '-54',
          showMv: false
        },
        {
          rank: 6,
          status: 'NEW',
          changed: 0,
          thumbnailImg: require('@/assets/image/temp/6.png'),
          title: '밝게 빛나는 별이 되어 비춰줄게',
          artist: '송이한',
          albumNm: '밝게 빛나는 별이 되어 비춰줄게',
          actionScore: 9999,
          actionChanged: '0',
          showMv: false
        },
        {
          rank: 7,
          status: 'RE_ENTRY',
          changed: 8,
          thumbnailImg: require('@/assets/image/temp/7.png'),
          title: 'Dolphin',
          artist: '오마이걸 (OH MY GIRL)',
          albumNm: 'NONSTOP',
          actionScore: 8500,
          actionChanged: '+26',
          showMv: false
        },
        {
          rank: 8,
          status: 'DOWN',
          changed: 6,
          thumbnailImg: require('@/assets/image/temp/8.png'),
          title: '에잇 (Prod. & Feat. SUGA of BTS)',
          artist: '아이유 (IU)',
          albumNm: '에잇',
          actionScore: 7652,
          actionChanged: '+86',
          showMv: false
        },
        {
          rank: 9,
          status: 'UP',
          changed: 25,
          thumbnailImg: require('@/assets/image/temp/9.png'),
          title: 'Love poem',
          artist: '아이유 (IU)',
          albumNm: 'Love poem',
          actionScore: 6512,
          actionChanged: '-2',
          showMv: false
        },
        {
          rank: 10,
          status: 'UP',
          changed: 11,
          thumbnailImg: require('@/assets/image/temp/10.png'),
          title: '딩가딩가 (Dingga)',
          artist: '마마무 (Mamamoo)',
          albumNm: '딩가딩가 (Dingga)',
          actionScore: 4521,
          actionChanged: '+54',
          showMv: false
        },
        {
          rank: 11,
          status: 'UP',
          changed: 540,
          thumbnailImg: require('@/assets/image/temp/1.png'),
          title: '밝게 빛나는 별이 되어 비춰줄게',
          artist: '송이한',
          albumNm: '밝게 빛나는 별이 되어 비춰줄게',
          actionScore: 6512,
          actionChanged: '-2',
          showMv: false
        },
        {
          rank: 12,
          status: 'UP',
          changed: 111,
          thumbnailImg: require('@/assets/image/temp/2.png'),
          title: 'Dolphin',
          artist: '오마이걸 (OH MY GIRL)',
          albumNm: 'NONSTOP',
          actionScore: 4500,
          actionChanged: '-21',
          showMv: false
        },
        {
          rank: 13,
          status: 'DOWN',
          changed: 5,
          thumbnailImg: require('@/assets/image/temp/3.png'),
          title: '에잇 (Prod. & Feat. SUGA of BTS)',
          artist: '아이유 (IU)',
          albumNm: '에잇',
          actionScore: 3000,
          actionChanged: '+86',
          showMv: false
        }
      ],
      faqList: [
        { show: false, q: '수집 채널의 기준은 무엇인가요?', a: '유튜브의 경우 공식 영상의 조회수와 좋아요 수를 대상으로 하며, SNS와 검색 엔진의 경우 일정 기간<br>내에 해당 트랙을 언급하는 모든 게시물과 포스팅을 수집 합니다.' },
        { show: false, q: '차트에 집계되는 곡의 기준이 무엇인가요?', a: '국내에서 발매된 음원의 스트리밍, 음원 판매량 상위곡을 대상으로 합니다. 따라서, 오래되어<br>인기가 없는 곡이나, 해외 음원은 집계되지 않습니다.' },
        { show: false, q: '점수는 어떻게 평가되나요?', a: '수집한 지표를 바탕으로 온라인 수집 채널 환경을 고려한 K-POP STATUS 만의 고유한 평가 모델을<br>적용하여 10000점 만점의 수치화 된 데이터로 평가 됩니다.' }
      ],
      isScrollDown: false,
      isDayList: true,
      weeks: [
        { weekNm: '55주차', key: 55, checked: false, isValid: false },
        { weekNm: '54주차', key: 54, checked: false, isValid: false },
        { weekNm: '53주차', key: 53, checked: false, isValid: true },
        { weekNm: '52주차', key: 52, checked: false, isValid: true },
        { weekNm: '51주차', key: 51, checked: false, isValid: true }
      ],
      calSelDt: '',
      highlightedFn: {
        dates: []
      },
      disabledDates: {
        date: []
      },
      selectedDate: new Date(), // 일자용
      openDate: new Date() // 월용
    }
  },
  computed: {
    date() {
      return this.$moment(this.selectedDate).format('YYYY.MM.DD')
    },
    month() {
      return this.$moment(this.openDate).format('YYYY.MM')
    },
    week() {
      return `${this.$moment(this.selectedDate).day(0).format('YYYY.MM.DD')}~${this.$moment(this.selectedDate).add(6, 'd').format('MM.DD')}`
    }
  },
  mounted() {

  },
  created() {
    document.addEventListener('scroll', this.watchScrollBehavior)
  },
  methods: {
    dayCellContent(e) {
      console.log(e);
      return e.date
    },
    watchScrollBehavior() {
      const nowScroll = Math.round(window.scrollY)
      if (nowScroll > 400) {
        this.isScrollDown = true
        document.querySelector('div-half-header').classList.add('showHeader')
      } else {
        this.isScrollDown = false
        document.querySelector('div-half-header').classList.remove('showHeader')
      }
    },
    onMoveToFaq() {
      const windowHeight = document.body.scrollHeight
      document.documentElement.scrollTop = windowHeight
    },
    onMoveToTop() {
      document.documentElement.scrollTop = 0
    },
    onShowMv(index) {
      const tempArr = this.albumList
      tempArr.forEach(item => {
        item.showMv = false
      })
      tempArr[index].showMv = true
      this.albumList = tempArr
    },
    handleAnimation(anim) {
      this.anim = anim
    },
    onClickAnywhere() {
      this.showCalendarView = false
      this.albumList.forEach(a => {
        a.showMv = false
      })
    },
    dateSelected(date) {
      this.highlightedFn.dates = []
      this.highlightedFn.dates.push(date)
      this.showCalendarView = false
      this.selectedDate = this.$moment(date)
    },
    weekSelected(index, valid) {
      if (valid) {
        this.showCalendarView = false
        this.isDayList = false
        this.weeks.forEach(i => {
          i.checked = false
        })
        this.weeks[index].checked = true
      }
    },
    onClickPrevBtn() { // 팝업 이전 월 선택버튼
      this.openDate = this.$moment(this.openDate).subtract(1, 'M')
    },
    onClickNextBtn() { // 팝업 다음 월 선택버튼
      this.openDate = this.$moment(this.openDate).add(1, 'M')
    },
    onChangeToPrev() { // 이전 선택 버튼
      if (this.isDayList) {
        this.selectedDate = this.$moment(this.selectedDate).subtract(1, 'd')
        this.openDate = this.$moment(this.selectedDate).subtract(1, 'd')
      } else {
        this.selectedDate = this.$moment(this.selectedDate).subtract(1, 'w')
        this.openDate = this.$moment(this.selectedDate).subtract(1, 'w')
      }
    },
    onChangeToNext() { // 다음 선택 버튼
      if (this.isDayList) {
        this.selectedDate = this.$moment(this.selectedDate).add(1, 'd')
        this.openDate = this.$moment(this.selectedDate).add(1, 'd')
      } else {
        this.selectedDate = this.$moment(this.selectedDate).subtract(1, 'w')
        this.openDate = this.$moment(this.selectedDate).subtract(1, 'w')
      }
    },
    onPrevMovie(index) {
      if (index === 0) {
        alert('이전 동영상이 없습니다.')
      } else {
        this.albumList[index].showMv = false
        this.albumList[index-1].showMv = true
      }

    },
    onNextMovie(index) {
      if (index === this.albumList.length - 1) {
        alert('다음 동영상이 없습니다.')
      } else {
        this.albumList[index].showMv = false
        this.albumList[index+1].showMv = true
      }
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
#main {
  @mixin font-style($size, $color) {
    font-size: $size; //  fallback
    color: $color;
  }
  @function rem($size) {
  $remSize: $size / 192.2;
  @return $remSize * 1rem;
  }
  $bk: #212529;
  $dk-gy: #595959;
  $lt-gy: #868E96;
  $red: #EA5743;
  $blue: #4499EE;
  $bg-gy: #EEEEEE;
  $bg-red: #F56049;
  // display: flex;
  // flex-direction: column;
   line-height: normal;

   .red-text {
      color: $red;
    }
    .blue-text {
      color:$blue;
    }
    .gray-text {
      color:#495057;
    }

    .invisible_background{
      width: 100%;
      height: 100vh;
      position: absolute;
      top: 0;
      left: 0;
    }
  *{
    font-family: NotoSans-Regular;
    }
    overflow-x: hidden;
  .header{
    .full_header{
      overflow: hidden;
      width: 100vw;
      background-color: white;
      text-align: center;
      color: white;
      .faq_button{
        cursor: pointer;
        position: absolute;
        top: rem(26);
        right: rem(360);
        width: rem(59);
        height: rem(34);
        border-radius: rem(10);
        font-size: rem(14);
        border: 1px solid rgba(white, 0.3);
        display: flex;
        align-items: center;
        justify-content: center;
      }
      .background-wrapper{
        width: 100%;
        height: 100%;
      }
      .center_text_wrapper{
        padding-top:   rem(100);
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        top: 0;
        .logo {
          width: rem(325);
        }
        .headline{
          font-size: rem(28);
          font-weight: 400;
          margin-top: rem(47);
        }
        dl {
          display: flex;
          align-items: center;
          justify-content: center;
          dt {
            margin: rem(26) rem(10) rem(34);
            img {
              width: rem(70);
            }
          }
        }
        .subline{
          font-size: rem(18);
          font-weight: 400;
        }
      }
    }
    .half_header.showHeader {
      margin-top: 0;

    }
    .half_header {
      z-index: 99;
      position: fixed;
      top: 0;
      left: 0;
      margin-top: -100px;
      width: 100%;
      background-color: rgba(white, 0.9);
      transition: margin 300ms ease;
      -webkit-box-shadow: 0px 4px 10px 0px rgba(226, 226, 226, 0.836);
      -moz-box-shadow: 0px 4px 10px 0px rgba(226, 226, 226, 0.836);
      box-shadow: 0px 4px 10px 0px rgba(226, 226, 226, 0.836);
      .inner {
        height: 85px;
        display: flex;
        width: 1199px;
        margin: 0 auto;
        align-items: center;
        .logo_wrapper {
          flex: 1;
          img {
            width: 159px;
            height: 39.98px;
          }
        }
        .faq_button{
          cursor: pointer;
          width: 59px;
          height: 34px;
          border-radius: 10px;
          font-size: 14px;
          border: 1px solid rgba(#212529, 0.2);
          display: flex;
          color: $bk;
          align-items: center;
          justify-content: center;
        }
      }
    }

  }
  .body {
    // flex-grow: 1;
    .body-inner-wrap{
      width: 1199px;
      margin: 0 auto;
      padding-bottom: 100px;
      .body_header{
        position: relative;
        margin-top: 94px;
        height: 74px;
        line-height: 74px;
        width: 100%;
        text-align: center;
        .tab_group{
          position: absolute;
          left:0;
          top: 0;
          width: 162px;
          display: flex;
          border: 1px solid #CED4DA;
          border-radius: 10px;
          height: 44px;
          line-height: 44px;
          overflow: hidden;
          dt{
            cursor: pointer;
            flex: 1;
            color: $dk-gy;
            font-size: 18px;
          }
          .on {
            background-color: $bk;
            color: white;
          }
          
        }
        .date_picker{
          display: flex;
          justify-content: center;
          width: max-content;
          margin: 0 auto;
          height: 40px;
          dt {
            display: flex;
            align-items: center;
          }
          .leftBtn, .rightBtn {
              width: 30px;
              cursor: pointer;
              background-repeat: no-repeat;
              background-position: 50% 50%;
              background-size:  9.26px auto;
          }
          .leftBtn {
            background-image: url('~@/assets/image/prev_btn_small.png');
            &:hover {
              background-image: url('~@/assets/image/prev_btn_small_r.png');
            }
          }
          .rightBtn {
            background-image: url('~@/assets/image/next_btn_small.png');
            &:hover {
              background-image: url('~@/assets/image/next_btn_small_r.png');
            }
          }
          .centerDt{
            flex-grow:1;
            justify-content: center;
            font-family:  Roboto-Regular;
            font-weight: 600;
            font-size: 24px;
            padding: 0 20px;
            cursor: pointer;
            p{
              flex-grow: 1;
              letter-spacing: 3px;
            }
            .cal_img {
              display: flex;
              align-items: center;              
              width: 19.65px;
              height: 19.65px;
              margin-left: 10px;
              background-repeat: no-repeat;
              background-position: 0;
              background-size:  19.65px auto;
              background-image: url('~@/assets/image/crdr_bk.png');
              

            }
            &:hover {
                .cal_img {
                  background-image: url('~@/assets/image/crdr_red.png');
                }
              }
          }
        }
          .calendar_view{
            width: 462px;
            height: max-content;
            z-index: 70;
            position: absolute;
            background-color: white;
            left: 600px;
            border: 1px solid #CED4DA;
            border-radius: 5px;
            padding: 20px 0 0;
            box-sizing: border-box;
            -webkit-box-shadow: 0px 4px 8px 0px rgba(196,196,196,0.63);
            -moz-box-shadow: 0px 4px 8px 0px rgba(196,196,196,0.63);
            box-shadow: 0px 4px 8px 0px rgba(196,196,196,0.63);
            font-size: 16px!important;
            font-family:  Roboto-Regular!important;

            .cal_header{
              display: flex;
              justify-content: center;
              margin: 0 auto;
              height: 60px;
              width: 100%;
              padding: 0 20px;
              padding-bottom: 20px;
              box-sizing: border-box;
              border-bottom: 1px solid #E8EAEC;
              position: relative;
              .today_sel_btn {
                position: absolute;
                left: 20px;
                top: 0;
                font-size: 14px;
                font-family:  NotoSans-Regular;
                width: 55px;
                height: 36px;
                border: 3px solid #CED4DA;
                border-radius: 10px;
                display: flex;
                align-items: center;
                justify-content: center;
                cursor: pointer;
              }
              .close_btn{
                cursor: pointer;
                img {
                  width: 17px;
                  position: absolute;
                  right: 20px;
                  top: 10px;
                }
              }
                  dt {
                display: flex;
                align-items: center;
              }
              .leftBtn, .rightBtn {
                  width: 30px;
                  cursor: pointer;
                  background-repeat: no-repeat;
                  background-position: 50% 50%;
                  background-size:  9.26px auto;
              }
              .leftBtn {
                background-image: url('~@/assets/image/prev_btn_small.png');
                &:hover {
                  background-image: url('~@/assets/image/prev_btn_small_r.png');
                }
              }
              .rightBtn {
                background-image: url('~@/assets/image/next_btn_small.png');
                &:hover {
                  background-image: url('~@/assets/image/next_btn_small_r.png');
                }
              }
              .centerDt{
                justify-content: center;
                font-family:  Roboto-Regular;
                font-weight: 600;
                font-size: 24px;
                padding: 0 20px;
                p{
                  flex-grow: 1;
                  letter-spacing: 3px;
                }
              }
            }

            .cal_body{
                display: flex;
                height: max-content;
                .week_sel{
                    height: 100%;
                    line-height: 140x;
                    padding-top: 20px;
                    text-align: center;
                    width: 100%;
                    padding-bottom: 20px;
                    .week_header{
                      font-size: 14px;
                      color: #ADB5BD;
                      height: 30px !important;
                      line-height: 30px!important;
                    }
                    .week_body{
                      margin: 0 auto;
                      font-size: 16px;
                      width: 68px;
                      text-align: center;
                      border-radius: 20px;
                      cursor: pointer;
                      height: 37px !important;
                      line-height: 37px!important;
                      margin-top: 10px;
                    }
                    .on {
                      background-color:#F56049 ;
                      color: white;
                    }
                    .off {
                      cursor: auto!important;
                      color: #ADB5BD;
                    }
                  }

            @supports not (display: grid) {
              .vdp-datepicker__calendar > div {
                 width: 100%;
                  display: -webkit-box;      /* OLD - iOS 6-, Safari 3.1-6 */
                  display: -moz-box;         /* OLD - Firefox 19- (buggy but mostly works) */
                  display: -ms-flexbox;      /* TWEENER - IE 10 */
                  display: -webkit-flex;     /* NEW - Chrome */
                  display: flex;
                  justify-content: space-between;
                  flex-wrap: wrap;

                div span:nth-child(7n - 6) {
                  margin-left: auto !important;
                }
                div span:nth-child(7n) {
                  margin-right: auto !important;
                }
                div span:last-child {
                  margin-left: 0 !important;
                  margin-right: auto !important;
                }

                .cell {
                  width: 30px!important;
                  height: 30px !important;
                  line-height: 30px!important;
                  vertical-align: middle !important;
                  margin: 0 10px !important;
                }
              }
            }

            @supports (display: grid) {
              .vdp-datepicker__calendar > div {
                display: grid;
                grid-template-columns: repeat(7, 1fr);
                justify-content: center;
                align-items: center;
                grid-auto-rows: 47px;
                .cell {
                  width: 100%!important;
                  justify-self: center;
                  align-self: center;
                }

              }
            }

                // 데이트피커 스타일
            .vdp-datepicker {
              width: 347px;
              padding: 10px 10px 20px;
              border-right: 1px solid #E8EAEC;

              .vdp-datepicker__calendar {
                width: 100%;
                border: unset;

                display: -webkit-box;      /* OLD - iOS 6-, Safari 3.1-6 */
                display: -moz-box;         /* OLD - Firefox 19- (buggy but mostly works) */
                display: -ms-flexbox;      /* TWEENER - IE 10 */
                display: -webkit-flex;     /* NEW - Chrome */
                display: flex;
                flex-direction: column;

                header {
                  display: none;
                }

                // 일자별 기본 스타일
                .cell {
                  padding: 0;
                  color: #000;
                  border: unset;
                  justify-self: center;
                  align-self: center;
                }

              // 요일 cell
              .day-header{
                font-size: 14px;
                color: #ADB5BD;
              }
              // 현재 선택한 날자 스타일
            .selected {
              background-color: unset !important;
              border: unset;
              border-radius: 50% !important;
              }

              .cell:not(.blank):not(.disabled).day:hover {
                border: unset;
              }

              .selected:focus {
                background-color: #DE6153 !important;
                border: unset !important;
                color: #fff;
                // line-height: 0.9444rem !important;
              }

              // 선택 불가 날짜 스타일
              .disabled {
                border-radius: 50% !important;;
                color: #ADB5BD !important;;
              }

              // 오늘 날짜 스타일
              .today {
                border: 3px solid $red !important;
                border-radius: 50%;
                width: 100%;
                height: 100%;
              }

              // 선택했던 날짜들 스타일
              .highlighted {
                background-color: $bg-red !important;
                border-radius: 50% !important;
                border: unset !important;
                color: #fff;
                height: 100%;
                line-height: 47px;
              }
            }
          }
      }

          }
      }
      .body_contents_wrapper{
        .column_wrapper{
          display: flex;
          padding: 16px 27px;
          box-sizing: border-box;
          width: 100%;
          border-top: 2px solid $bk;
          border-bottom: 1px solid #E8EAEC;
          dt{
            font-size: 12px;
            color:$lt-gy;
            &:nth-child(1) {flex: 7.75%;}
            &:nth-child(2) {flex: 62%;}
            &:nth-child(3) {flex: 21.3%;}
            &:nth-child(4) {flex: 3.7%;}
          }
        }
        .list-wrapper{
          position: relative;
          padding: 20px 27px;
          box-sizing: border-box;
          display: flex;
          width: 100%;
          dt {
            &:nth-child(1) {flex: 7.75%;}
            &:nth-child(2) {flex: 62%;}
            &:nth-child(3) {flex: 21.3%;}
            &:nth-child(4) {flex: 3.7%;}
          }
          .rank {
            display: flex;
            h1{
              display: flex;
              align-items: center;
              font-family:  Roboto-Bold;
              font-size: 20px;
              width: 40px;

            }
            .upanddown_wrapper{
              flex-grow: 1;
              font-family:  Roboto-Regular;
              font-size: 14px;
              text-align: center;
              margin: auto 0;
              img{
                width: 8px;
              }
            }
          }
          .artist_info{
            cursor: pointer;
            display: flex;
            .img_wrap{
              img{
                width: 72px;
              }
            }
            .right_name_wrap{
              box-sizing: border-box;
              padding-left: 25px;
              display: flex;
              align-items: center;
              .inner {
                .title{
                  font-size: 16px;
                  margin-bottom: 10px;
                }
                .artist{
                  font-size: 14px;
                  color:$lt-gy;
                }
              }

            }
        }
        .album_info{
            display: flex;
            align-items: center;
            p{
              font-size: 14px;
                color:$lt-gy;
            }
          }
          .action_score{
            font-family: Roboto-Italic;
            text-align: center;
            display: flex;
            align-items: center;
            .inner {
              h1{
                font-size: 18px;
              }
              .upanddown_wrapper{
                font-size: 14px;
                background-color: $bg-gy;
                border-radius: 30px;
                margin-top: 8px;
                width: 50px;
                height: 26px;
                line-height: 26px;
                overflow: hidden;
              }
            }
          }

          &:hover {
            background-color: #FBFBFB;
          }

          // 팝업
          .movie_popup {
            z-index: 60;
            position: absolute;
            background-color: white;
            top: 100px;
            left : 200px;
            border: 1px solid #CED4DA;
            border-radius: 5px;
            padding: 20px;
            box-sizing: border-box;
            -webkit-box-shadow: 0px 4px 8px 0px rgba(196,196,196,0.63);
            -moz-box-shadow: 0px 4px 8px 0px rgba(196,196,196,0.63);
            box-shadow: 0px 4px 8px 0px rgba(196,196,196,0.63);

            .close_btn{
              cursor: pointer;
              position: absolute;
              top: -60px;
              right: 5px;
              img {
                width: 45px;
              }
            }
            .popup_header_area{
              display: flex;
              padding: 20px 0 40px;
              .prev_btn, .next_btn{
                cursor: pointer;
                width: 23.96px;
                height: 47.93px;
              }
              .title_wrapper{
                flex-grow: 1;
                text-align: center;
                .title{
                  font-size: 22px;
                }
                .artist{
                  margin-top: 12px;
                  font-size: 18px;
                  color: $lt-gy;
                }
              }
            }
            .popup_content_area{

            }
          }
        }

      }
    }
  }
  .footer {
    width: 100%;
    min-height: 534px;
    background-color: #F5F5F5;
    display: flex;
    flex-direction: column;

    .faq_wrapper {
      flex-grow: 1;
      margin: 0 auto;
      width: 1199px;
      display: flex;
      color: $bk;
      margin-top: 120px;
      padding-bottom: 100px;
      .faq_left{
        width: 378px;
        color: $dk-gy;
        font-family: NotoSans-Regular;
        padding-top: 40px;
        .title {
          font-weight: 500;
          font-size: 20px;
          margin-bottom: 15px!important;
          padding-left: 5px;
        }
        .cs_number {
          font-family: NotoSans-Regular;
          font-size: 32px;
          font-weight: 700;
          margin-bottom: 10px!important;
        }
        .operating_time{
          font-size: 20px;
          margin-bottom: 20px!important;
        }
        .req_btn{
          cursor: pointer;
          width: 150px;
          height: 40px;
          line-height: 40px;
          border-radius: 5px;
          background: $dk-gy;
          padding: 0 30px;
          box-sizing: border-box;
          background-image: url('~@/assets/image/path_w.png');
          background-repeat: no-repeat;
          background-position: 80% 50%;
          background-size:  5px 10px;
          p{
            color: white;
            font-size: 18px;
            font-weight: 600;
          }
        }
      }
      .faq_list_wrapper{
        flex-grow: 1;
        .faq_text{
          font-family: NotoSans-Bold;
          font-size: 32px;
          padding-left: 30px;
          margin-bottom: 20px;
        }
        .question_wrapper{
          cursor: pointer;
          box-sizing: border-box;
          border-bottom: 1px solid #CED4DA;
          p{
            font-size: 18px;
          }
          &:nth-child(1) {
            border-top: 1px solid $bk;
          }
          div {
            background-repeat: no-repeat;
            background-position: 20px 50%;
            background-size:  28px 28px;
            padding-left: 58px;
          }
          .question {
            background-image: url('~@/assets/image/icon_Q.png');
            height: 30px;
            line-height: 30px;
            padding-top: 25px;
            padding-bottom: 25px;
          }
          .answer{
            background-image: url('~@/assets/image/icon_A.png');
            padding-top: 18px;
            padding-bottom: 18px;
            background-color: white;
            border-radius: 10px;
            margin-bottom: 25px;
          }
        }
      }
    }
    .very_last_footer{
      height: 81px;
      line-height: 81px;
      width: 100%;
      background-color: $bk;
      font-family: Roboto-Regular;
      text-align: center;
      color: white;
    }
  }
  .move-top-btn{
    cursor: pointer;
    position: fixed;
    bottom: 100px;
    right: 170px;
    border-radius: 50px;
    background-color: $bk;
    font-family: Roboto-Regular;
    text-align: center;
    color: white;
    width: 45px;
    height: 45px;
    line-height: 45px;
    font-size: 12px;
    background-image: url('~@/assets/image/path_top.png');
    background-repeat: no-repeat;
    background-position: 50% 20%;
    background-size:  7.8px 3.9px;
    p{
      padding-top: 2px;
    }

  }
}
</style>
