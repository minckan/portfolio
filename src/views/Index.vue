<template>
  <div id="main">
    <div class="invisible_background"/>
    <section class="header">
      <!-- full header -->
      <div class="full_header">
        <div class="background-wrapper">
          <lottie :options="defaultOptions" @animCreated="handleAnimation"/>
        </div>

        <!-- <dl class="menu">
          <dt>experiences</dt>
          <dt>skills</dt>
        </dl> -->
        <div class="center_text_wrapper">
          <div class="me"><img  src="@/assets/images/me.jpg" alt="강민주"></div>
          <p class="headline">
            안녕하세요, 
            웹 프론트엔드 개발과 앱개발을 같이 하고있는<br>
            개발자 강민주 입니다
          </p>
          
          <dl>
            <dt><img src="@/assets/images/GitHub-Mark-Light-64px.png" alt="" @click="openUrl('https://github.com/minckan?tab=repositories')"></dt>
          </dl>
          <p class="subline">minc0317@daum.net</p>
        </div>
      </div>

      <!-- small-header -->
      <div id="div-half-header" class="half_header">
        <div class="inner">
          <p>FE & App Developer</p>
          <p>Kang Min Ju</p>
        </div>
      </div>

    </section>

    <section class="body">
      <div class="inner_body_wrap">
        <!-- experiences -->
       <section>
         <div>
          <p class="title">Experiences</p>
          <dl class="project_list_wrap">
            <dt @click="$router.push({name: 'page', params: {idx: '0'}})"><input v-model="selected" type="radio" name="radio" value="0" id="stampit" :checked="selected === '0'"><label for="stampit">Stampit</label></dt>
            <dt @click="$router.push({name: 'page', params: {idx: '1'}})"><input v-model="selected" type="radio" name="radio" value="1" id="mp" :checked="selected === '1'"><label for="mp">미스터피자</label></dt>
            <dt @click="$router.push({name: 'page', params: {idx: '3'}})"><input v-model="selected" type="radio" name="radio" value="3" id="dds" :checked="selected === '3'"><label for="dds">DDS</label></dt>
            <dt @click="$router.push({name: 'page', params: {idx: '2'}})"><input v-model="selected" type="radio" name="radio" value="2" id="makestar" :checked="selected === '2'"><label for="makestar">MakeStar</label></dt>
            <!-- <dt><input v-model="selected" type="radio" name="radio" value="4" id="shrimp"><label for="shrimp">ShrimpPic</label></dt> -->
          </dl>
          <!-- STAMPIT -->
          <div v-if="selected === '0'" class="contents stampit two_wrapper">
            <div class="left_contents">
             <div class="video_view_wrapper">
                <div class="video_view">
                <video controls width="100%" height="100%" poster="@/assets/images/stampit.png">
                  <source src="@/assets/Stampit_record.mp4"
                          type="video/mp4">
                  Sorry, your browser doesn't support embedded videos.
                </video>
              </div>
             </div>
            </div>
            <div class="right_contents">
              <img class="logo" src="@/assets/images/symbol_manager.png" alt="">
              <dl class="platform-list">
                <dt>Android</dt>
                <dt>iOS</dt>
              </dl>
              <p class="app-intro">벨기에 소재 푸드코트 쿠폰앱</p>
              <p class="sub-title">담당 작업</p>
              <hr>
              <p class="functions">1. 전체 화면 개발 : UI개발, API 연동, 스탬프 SDK 연동 
                <br>2. 안드로이드&middot;iOS개발 : FCM, APNs, 소셜로그인 등
                <br>3. 앱 런칭 및 유지보수
              </p>
              <p class="sub-title">사용기술</p>
              <hr>
              <p class="functions">vue.js / ionic / android(java) / iOS(swift) / AWS / Firebase</p>

              

              <div class="download_btn">
                <img src="@/assets/images/btn_google_play.png" alt="goole paly store" @click="openUrl('https://play.google.com/store/apps/details?id=com.stampit.app')">
                <img src="@/assets/images/btn_app_store.png" alt="apple store" @click="openUrl('https://apps.apple.com/us/app/stampit-by-plusdeals/id1619061573')">
              </div>
            </div>
            

          </div>

          <!-- Mr Pizza -->
          <div v-if="selected === '1'" class="contents mrpizza ">
            <section class="two_wrapper">
              <div class="left_contents">
                <div class="video_view_wrapper">
                  <div class="video_view">
                    <video controls width="100%" height="100%" poster="@/assets/images/mrpizza.png">
                      <source src="@/assets/MP_record.mp4"
                              type="video/mp4">
                      Sorry, your browser doesn't support embedded videos.
                    </video>
                  </div>
                </div>
              </div>
              <div class="right_contents">
                <img class="logo" src="@/assets/images/mp-logo.png" alt="">
                <dl class="platform-list">
                  <dt>Android</dt>
                  <dt>iOS</dt>
                  <dt>Mobile-Web</dt>
                  <dt>PC-Web</dt>
                </dl>
                <p class="app-intro">미스터 피자 리뉴얼 버전 앱과 웹</p>
                <p class="sub-title">담당 작업</p>
                <hr>
                <p class="functions">1. 화면 개발 : 전체 UI개발, API 연동, 결제모듈 연동
                  <br>2. 안드로이드&middot;iOS개발 : FCM, APNs, 소셜로그인
                  <br>3. 앱 런칭
                </p>
                <p class="sub-title">사용기술</p>
                <hr>
                <p class="functions">vue.js / android(java) / iOS(swift) / AWS / Firebase</p>
              </div>

            </section>
            <p class="pcVer">PC Ver.</p>
            <img class="screenshots" src="@/assets/images/mp_pc_screenshot.png" alt="">
            <p class="aboutToPublish">출시예정</p>
          </div>


          <!-- 메이크스타 -->
          <div v-if="selected === '2'" class="contents makestar">
            <section class="two_wrapper">
              <div class="left_contents">
                <div class="video_view_wrapper">
                  <div class="video_view">
                    <video controls width="100%" height="100%" poster="@/assets/images/makestar_thumbnail.jpeg">
                      <source src="@/assets/Makestar_record.mp4"
                              type="video/mp4">
                      Sorry, your browser doesn't support embedded videos.
                    </video>
                  </div>
                </div>
              </div>
              <div class="right_contents">
                <img class="logo" src="@/assets/images/makestar-logo.svg" alt="">
                <dl class="platform-list">
                  <dt>Mobile-Web</dt>
                  <dt>PC-Web</dt>
                </dl>
                <p class="app-intro">클라우드펀딩 기반 엔터테인먼트 콘텐츠 플랫폼</p>
                <p class="sub-title">담당 작업</p>
                <hr>
                <p class="functions">1. 화면 개발 : UI개발, API 연동, 다국어적용</p>
                <p class="sub-title">사용기술</p>
                <hr>
                <p class="functions">nuxt.js</p>
                
                <div  class="link">
                  <a href="https://www.makestarcorp.com/">메이크스타 둘러보기</a>
                </div>
              </div>
            </section>
            <!-- <img class="screenshots" src="@/assets/images/makestar_pc_screenshot.png" alt=""> -->
           
          </div>
                  <!-- dds -->
          <div v-if="selected === '3'" class="contents dds">
            <section class="two_wrapper">
               <div class="left_contents">
                <div class="video_view_wrapper">
                  <div class="video_view">
                    <video controls width="250" poster="@/assets/images/dds.png">
                      <source src="@/assets/DDS_record.mp4"
                              type="video/mp4">
                      Sorry, your browser doesn't support embedded videos.
                    </video>
                  </div>
                </div>
               </div>
              <div class="right_contents">
                <img class="logo" src="@/assets/images/dds-logo.png" alt="">
                <dl class="platform-list">
                  <dt>Android</dt>
                  <dt>iOS</dt>
                  <dt>Mobile-Web</dt>
                  <dt>PC-Web</dt>
                </dl>
                <p class="app-intro">외국인을 대상으로 한 대구 관광 쇼핑 앱</p>
                <p class="sub-title">담당 작업</p>
                <hr>
                <p class="functions">1. 화면 개발 : UI개발, API 연동, 결제모듈 연동, 다국어 적용, 소셜로그인 
                  <br>2. 안드로이드&middot;iOS개발 : FCM, APNs, 소셜로그인
                  <br>3. 앱 런칭 
                </p>
                <p class="sub-title">사용기술</p>
                <hr>
                <p class="functions">vue.js/ ionic / android(java) / iOS(swift) / AWS / Firebase</p>
              </div>
            </section>
            <p class="pcVer">PC Ver.</p>
            <img class="screenshots" src="@/assets/images/dds_pc_screenshot.png" alt="">
            <p class="aboutToPublish">출시예정</p>
            
          </div>
         </div>
  
       </section>
        <!-- skills -->
       <section>
         <div>
          <p class="title">Skills</p>
         </div>
        <dl class="skill_section">
          <dt><img src="@/assets/images/skills/vue.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/js.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/android.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/apple.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/swift.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/react.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/ts.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/firebase.png" alt=""></dt>
          <dt><img src="@/assets/images/skills/sass.png" alt=""></dt>
        </dl>
       </section>
      </div>
    </section>

  
    <!-- <div v-if="isScrollDown" class="move-top-btn" @click="onMoveToTop()">
      <p>TOP</p>
    </div> -->

    <div class="footer">
      <p>ⓒ 2022. minckan all rights reserved.</p>
    </div>

  </div>
</template>

<script>
import Lottie from 'vue-lottie'
import animationData from '@/assets/lottie/motion.json'

export default {
  name: 'Main',
  components: {
    Lottie
  },
  filters: {
    
  },
  data() {
    return {
      defaultOptions: { animationData: animationData },
     isScrollDown: false,
     selected: this.$route.params.idx || '0'
    }
  },
   computed: {
     
    },
  mounted() {

  },
  created() {
    document.addEventListener('scroll', this.watchScrollBehavior)
  },
  methods: {
        handleAnimation(anim) {
      this.anim = anim
    },
   openUrl(url) {
      window.location.href = url
    },
    watchScrollBehavior() {
    const nowScroll = Math.round(window.scrollY)
    if (nowScroll > 100) {
      this.isScrollDown = true
      document.getElementById('div-half-header').classList.add('showHeader')
    } else {
      this.isScrollDown = false
      document.getElementById('div-half-header').classList.remove('showHeader')
    }
  },
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

   * {
     font-family: 'Roboto','Noto Sans KR', sans-serif;
   }

   .red-text {
      color: $red;
    }
    .blue-text {
      color:$blue;
    }
    .gray-text {
      color:#495057;
    }

    // .invisible_background{
    //   width: 100%;
    //   height: 100vh;
    //   position: absolute;
    //   top: 0;
    //   left: 0;
    // }
    overflow-x: hidden;
  .header{
    .full_header{
      overflow: hidden;
      width: 100vw;
      background-color: white;
      text-align: center;
      color: white;
      .menu {
        position: absolute; top: 20px; right: 100px; display: flex;
        dt {
          margin-left: rem(50); font-size: rem(24); cursor: pointer;
        }
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
        .me {
          width: rem(100); height: rem(100); display: flex; align-items: center; justify-content: center;
          border-radius: 50%; overflow: hidden; margin: 0 auto; border: 5px solid white;
          img {
            height: 100%; 
          }
        }
        .headline{
          font-size: rem(28);
          font-weight: 400;
          margin-top: rem(27);
        }
        dl {
          display: flex;
          align-items: center;
          justify-content: center;
          dt {
            margin: rem(26) rem(10) rem(34); cursor: pointer;
            img {
              width: rem(50);
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
        justify-content: space-between;
        
        .menu {
          display: flex;
           dt {
             margin-left: rem(50); font-size: rem(24); cursor: pointer; color: #1d1d1d;
           }
         }
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
    background: white; margin-top: -20px;
    .inner_body_wrap{
      section {
        // min-height: 500px;
        padding: 50px 0;
        div {
           width: 1190px; margin: 0 auto;
          .title {
            font-family: 'Roboto', sans-serif; font-size: 42px; color: black; font-weight: 700;
          }
        }
        &:nth-child(2n) {
          // background: #F4F4FB;
        }

        &:nth-child(1) {
          min-height: 800px;
          .project_list_wrap {
            display: flex; justify-content: space-around; align-items: center;  width: 70%; margin: 0 auto; margin-top: 40px;
            dt {
               border-radius: 20px;padding: 0 20px;  height: 30px;flex: 1;
              display: flex;align-items: center; justify-content: center; 
              input {
                appearance: none;
              }
              input + label {
                 color: lightgray; font-size: 16px; cursor: pointer;
              }
              input:checked + label {
                font-size: 20px; font-weight: bold; color: tomato; font-weight: 700; border-bottom: 2       px solid tomato;
              }
            }
          }
          .contents {
            display: flex; margin: 100px 0; align-items: center; flex-direction: column;
            .logo {
              width: 150px;;
            }
            .platform-list {
              display: flex; margin-top: 20px;
              dt {
                 font-size: 16px;padding: 0 10px; border-radius: 10px; margin-right: 5px;
                 &:last-child {
                   margin-right: 0;
                 }
              }
            }
            .app-intro {
              margin-top: 20px;
            }
            .sub-title {
              font-size: 16px; color: gray; margin-top: 50px; 
            }
            hr {
              width: 20px; margin: 20px 0; 
            }
            .functions {
              text-align: center; font-size: 20px;
            }
            .download_btn {
             display: flex; justify-content: center; margin-top: 50px;
             img {
               width: 200px; margin-right: 20px; cursor: pointer;
               &:last-child {
                 margin: 0;
               }
             }
            }
            .video_view {
              display: flex; justify-content: center; margin-top: 50px;
            }
            .screenshots {
              width: 700px; margin-bottom: 50px;
            }
            .aboutToPublish {
               font-size: 16px; color: gray;
            }
          }
          .link {
            margin-top: 30px;
          }
          .pcVer {
            margin-top: -90px; margin-bottom: 20px; font-size: 16px;
          }
        
          .two_wrapper {
            flex-direction: row; display: flex; width: 1190px; padding-bottom: 0;
            height: max-content; align-items: flex-start; padding-top: 0;
            .left_contents {
              width: max-content;  padding-left: 130px;
              .video_view_wrapper {
                width: 300px; position: relative; height: 650px; 
                background-image: url('~@/assets/images/phone.png');
                background-repeat: no-repeat;
                background-size: 100% 100%; margin-right:60px ; 
                .video_view { 
                  margin: 0; width: 260px; position: absolute; left: 50%; transform: translateX(-50%);
                  top: 90px; overflow: hidden; height: 470px;

                  video[poster]{
                  height:100%;
                  width:100%;
                  object-fit: fill;
                  }
                }
              }
            }
            .right_contents {
              flex: 1;
              .functions {
                text-align: left;
              }
              .download_btn {
                justify-content: flex-start;
              }
            }
          }
          .stampit {
              .platform-list {
                dt {
                  border: 1px solid #3CB4EF; color: #3CB4EF;
                }
              }
            }
          .mrpizza {
              .platform-list {
                dt {
                  border: 1px solid #EF3F3E; color: #EF3F3E;
                }
              }
            }
            .makestar {
              .logo {
                height: 150px;
              }
              .platform-list {
                dt {
                  border: 1px solid #e8351d; color: #e8351d;
                }
              }
            }
            .dds {
              .platform-list {
                dt {
                  border: 1px solid #22BDB6; color: #22BDB6;
                }
              }
            }
        }
        &:nth-child(2) {
          .skill_section {
            display: grid; grid-template-columns: repeat(5, 1fr);
            width: 1199px; justify-items: center; margin: 0 auto;
            margin-top: 50px; gap: 30px;
           dt {
             flex: 1; align-self: center;
              img {
                 width: 100px;
              }
           }
          }
        }
        &:nth-child(3) {}

      }
    }
  }

  .footer {
    width: 100%; height: 100px; background: #EEEEEE;
    display: flex; align-items: center;
    p {
      width: 1190px; margin:  0 auto; font-size: 16px; color: gray;
      text-align: center;
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
    // background-image: url('~@/assets/image/path_top.png');
    background-repeat: no-repeat;
    background-position: 50% 20%;
    background-size:  7.8px 3.9px;
    p{
      padding-top: 2px;
    }

  }
}
</style>
