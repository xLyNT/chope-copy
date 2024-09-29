<script setup lang="ts">
import Select from '../controls/Select.vue';
import {computed, ref} from "vue";
import '../styles/style.css'
import ManIcon from "../assets/manIcon.svg";
import EmailIcon from "../assets/EmailIcon.png";
import PhoneInput from "../controls/PhoneInput.vue";
import textareaIcon from "../assets/textareaIcon.png"
import calendarIcon from "../assets/calendarIcon.svg"
import personsIcon from "../assets/personsIcon.png"
import placementIcon from "../assets/placementIcon.svg"
import clockIcon from "../assets/clockIcon.svg"
import SelectWithSelected from "../controls/SelectWithSelected.vue";

const language = ref('English');
const stage = ref(1);

// const adults = ref('');
// const date = ref(null);
const firstName = ref('');
const lastName = ref('');
const email = ref('');
const placement = ref('');
const phone = ref('');
const agreement = ref(false);

const usePromo = ref(false);
const code = ref('');

const updates = ref(false);
const recommendations = ref(false);
const selectValues = ref({
  adults: '1 Adult',
  children: '0 Children'
});
const comment = ref('');
const shareEmail = ref('');

const computedDate = computed(() => {
  return 'Saturday, September 21, 2024 at 5:45 PM'
})

function handleFirstStage() {
  if (!agreement.value) return;
  stage.value = 2;
}
function handleSecondStage(){
  stage.value = 3
}

function handleSelect(key: 'adults' | 'children', value: any) {
  selectValues.value[key] = value.value;
}
</script>

<template>
  <div class="container">
    <div class="header">
      <div class="line">
        <div class="language-picker">
          {{ language }}
          <div class="arrow-down"></div>
        </div>
      </div>
      <div class="banner">

      </div>
    </div>
    <div class="content">
      <div class="steps-container">
        <div class="steps-horizontal">
          <div class="step" :class="stage >= 1 ? 'active' : ''">
            <div class="step__head">
              <div class="step__line">
              </div>
              <div class="step__icon">
                <div class="step__icon-inner">
                  1
                </div>
              </div>
            </div>
            <div class="step__main">
              <div class="step__title">Reservation</div>
            </div>
          </div>
          <div class="step" :class="stage >= 2 ? 'active' : ''">
            <div class="step__head">
              <div class="step__line">
              </div>
              <div class="step__icon">
                <div class="step__icon-inner">
                  2
                </div>
              </div>
            </div>
            <div class="step__main">
              <div class="step__title">Information</div>
            </div>
          </div>
          <div class="step" :class="stage >= 3 ? 'active' : ''">
            <div class="step__head">
              <div class="step__line">
              </div>
              <div class="step__icon">
                <div class="step__icon-inner">
                  3
                </div>
              </div>
            </div>
            <div class="step__main">
              <div class="step__title">Confirmation</div>
            </div>
          </div>
        </div>

      </div>
      <div class="warning">

      </div>
      <div class="form">
        <div class="form-stage" v-if="stage === 1">
          <div class="inputs-container">
            <div class="select-container">
              <Select :image="ManIcon" @select="handleSelect('adults',$event)"
                      :list="[{title:'1 Adult',value:'1 Adult'},{title:'2 Adults',value:'2 Adults'},{title:'3 Adults',value:'3 Adults'},{title:'4 Adults',value:'4 Adults'},{title:'5 Adults',value:'5 Adults'},{title:'6 Adults',value:'6 Adults'},{title:'7 Adults',value:'7 Adults'},{title:'8 Adults',value:'8 Adults'}]"/>
            </div>
            <div class="select-container">
              <Select :image="ManIcon" @select="handleSelect('children',$event)"
                      :list="[{title:'0 Children',value:'0 Children'},{title:'1 Child',value:'1 Child'},{title:'2 Children',value:'2 Children'},{title:'3 Children',value:'3 Children'},{title:'4 Children',value:'4 Children'}]"/>
            </div>
            <div class="select-container">
              <Select :image="ManIcon"
                      :list="[{title:'1 Adult',value:'1 Adult'},{title:'2 Adults',value:'2 Adults'},{title:'3 Adults',value:'3 Adults'},{title:'4 Adults',value:'4 Adults'},{title:'5 Adults',value:'5 Adults'},{title:'6 Adults',value:'6 Adults'},{title:'7 Adults',value:'7 Adults'},{title:'8 Adults',value:'8 Adults'}]"/>
            </div>
            <div class="select-container">
              <SelectWithSelected :image="clockIcon"
                      :list="[{title:'8:00 am',value:'8:00 am'},{title:'8:15 am',value:'8:15 am'},{title:'8:30 am',value:'8:30 am'},{title:'8:45 am',value:'8:45 am'},
                      {title:'9:00 am',value:'9:00 am'},{title:'9:15 am',value:'9:15 am'},{title:'9:30 am',value:'9:30 am'},{title:'9:45 am',value:'9:45 am'},
                      {title:'10:00 am',value:'10:00 am'},{title:'10:15 am',value:'10:15 am'},{title:'10:30 am',value:'10:30 am'},{title:'10:45 am',value:'10:45 am'}]"/>
            </div>
            <div class="floor-container">
              <SelectWithSelected :image="placementIcon"
                      :list="[{title:'First Floor',value:'First Floor'},{title:'Second Floor',value:'Second Floor'}]"/>
            </div>
          </div>
          <div class="text-block">
            <div class="heading">
              Smoking is not allowed in any area of the restaurant.
            </div>
            <div class="text">
              Special requests are not guaranteed and are subject to availability and restaurant discretion. Notes
              stating an alternative timing or party-size will not be accommodated.
            </div>
          </div>
          <div class="terms-agreement">
            <input type="checkbox" name="agree" class="checkbox" v-model="agreement" id="agree"/>
            <label for="agree">I have read and agree to the above terms and conditions.</label>
          </div>
          <button class="button-next" :class="agreement ? 'allow' : ''" @click="handleFirstStage">Next</button>
        </div>
        <div class="form-stage" v-else-if="stage === 2">
          <div class="inputs-container">
            <div class="input-line">
              <div class="select-container">
                <Select :image="ManIcon"
                        :list="[{title:'Mr.',value:'Mr.'},{title:'Ms.',value:'Ms.'},{title:'Mrs.',value:'Mrs.'},{title:'Mdm.',value:'Mdm.'},{title:'Dr.',value:'Dr.'},{title:'Mx.',value:'Mx.'}]"/>
              </div>
            </div>
            <div class="input-line">
              <div class="input-container">
                <span class="required">*</span>
                <div class="input">
                  <img :src="ManIcon" alt=""/>
                  <input type="text" class="text-input" placeholder="First Name" v-model="firstName"/>
                </div>
              </div>
              <div class="input-container">
                <div class="input">
                  <input type="text" class="text-input" placeholder="Last Name" v-model="lastName"/>
                </div>
              </div>
            </div>
            <div class="input-line">
              <div class="input-container full-w">
                <span class="required">*</span>
                <div class="input">
                  <img :src="EmailIcon" alt="" class="email-icon"/>
                  <input type="email" class="text-input" placeholder="Email Address" v-model="email"/>
                </div>
              </div>
            </div>
            <div class="input-line">
              <div class="input-container full-w">
                <span class="required">*</span>
                <PhoneInput v-model="phone"/>
              </div>
            </div>
            <div class="input-line">
              <textarea class="textarea" placeholder="Message (Maximum 85 characters.)" v-model="comment"></textarea>
            </div>
            <div class="input-line">
              <div class="input-container full-w">
                <div v-if="usePromo" class="input">
                  <label for="code">Promo Code</label>
                  <input type="text" name="code" placeholder="Code" class="text-input code-input" autocomplete="off"
                         v-model="code"/>
                  <button type="button" class="btn" :class="code.length > 2 ? 'active' : ''">Apply</button>
                </div>
                <div v-else class="use-promo" @click="usePromo = true">
                  Use a promotion code
                </div>
              </div>


            </div>
            <div class="input-line">
              <div class="checkbox-container">
                <input type="checkbox" name="updates" class="checkbox" id="updates" v-model="updates"/>
                <label class="stage-two" for="updates">Yes, I'd love to get updates, news and promotions from this
                  restaurant!</label>
              </div>
            </div>
            <div class="input-line">
              <div class="checkbox-container">
                <input type="checkbox" name="recommendations" class="checkbox" id="recommendations"
                       v-model="recommendations"/>
                <label class="stage-two" for="recommendations">I’d love to receive personalised dining recommendations,
                  deals, and invitations from Chope!</label>
              </div>
            </div>
          </div>
          <div class="controls">
            <button type="button" class="el-btn back-btn" @click="stage = 1">
              <span>Back</span>
            </button>
            <button type="button" class="el-btn confirm-btn" @click="handleSecondStage">
              <span>Confirm Booking</span>
            </button>
          </div>
          <div class="terms">
            <p>By confirming this reservation, I agree to Chope's <a style="color: #428bca;text-decoration:none"
                                                                     href="https://www.chope.co/bali-restaurants/tc"
                                                                     target="_self">Privacy Policy and Terms &amp;
              Conditions.</a></p>
          </div>
        </div>
        <div class="form-stage" v-else-if="stage === 3">
          <div class="heading">
            <p><b>Your Reservation is Confirmed!</b> Reservation details have been sent to <b>{{ email }}</b></p>
          </div>
          <div class="reservation">
            <h4> Confirmation ID: <span>{{ 'CONFID' }}</span></h4>
            <div class="reservation-data">
              <div class="reservation-line">
                <img :src="ManIcon" alt="" class="reservation-icon">
                <span>{{ firstName + ' ' + lastName }}</span>
              </div>
              <div class="reservation-line">
                <img :src="calendarIcon" alt="" class="reservation-icon">
                <span>{{ computedDate }}</span>
              </div>
              <div class="reservation-line">
                <img :src="personsIcon" alt="" class="reservation-icon">
                <span>{{
                    selectValues.adults + (selectValues.children[0] === '0' ? '' : ', ' + selectValues.children)
                  }}</span>
              </div>
              <div class="reservation-line">
                <img :src="textareaIcon" alt="" class="reservation-icon">
                <span>{{ comment }}</span>
              </div>
              <div class="reservation-line">
                <img :src="placementIcon" alt="" class="reservation-icon">
                <span>{{ placement || 'Terrace' }}</span>
              </div>
              <div class="cancel-line">
                <span class="link">Click here</span>
                <span class="text">to EDIT/CANCEL your reservation</span>
              </div>
            </div>
          </div>
          <div class="sharing">
            <p class="solids"></p>
            <p class="invite"> Invite your friends: </p>
            <div class="input-container">
              <div class="input">
                <img :src="EmailIcon" alt="" class="email-icon"/>
                <input type="email" class="text-input" placeholder="Email Address" v-model="shareEmail"/>
              </div>
            </div>
            <button type="button" class="el-btn share-btn" :class="shareEmail.length ? 'active' : ''"><span>Share</span>
            </button>
            <div class="download-app">
              <p class="invite"> Make your next booking even easier - download Chope's mobile app today! </p>
              <div class="store">
                <a href="https://go.onelink.me/1953970613?pid=Widget&amp;c=NonChope_Source">
                  <img src="https://static.chope.co/static/widget_v5/img/google-play.png?date=20190429" alt="">
                </a>
                <a href="https://go.onelink.me/1953970613?pid=Widget&amp;c=NonChope_Source">
                  <img src="https://static.chope.co/static/widget_v5/img/appstore.png?date=20190429" alt="">
                </a>
              </div>
              <p class="done"> You're done! To see recommended dishes at this restaurant
                <a href="https://www.chope.co/bali-restaurants/restaurant/brie-restaurant-and-cheesery-ubud#DISHES_WE_LOVE">click here</a></p>
            </div>
          </div>
        </div>
      </div>
      <div class="footer">
        <span>
          <a href="https://www.chope.co/bali-restaurants" target="_blank" class="foot_power_a">  - www.chope.co </a>
          <i>Reservations powered by Chope</i>
          <br>
          <span>© 2024 Chope, All rights reserved. </span>
          <a target="_blank" href="https://www.chope.co/bali-restaurants/tc" class="foot_power">
            <em>Terms and Conditions</em>
          </a>
          <br>
        </span><div class="center"></div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
  gap: 24px;
  max-width:498px;
  width:100%;
  background: #fff;
  border: 1px solid #d5d5d5;
  border-radius: 3px;
  padding-bottom: 25px;
  color: #000;

  .header {
    .line {
      display: flex;
      justify-content: flex-end;
      align-items: center;
      background: #000;

      .language-picker {
        display: flex;
        gap: 8px;
        font-size: 12px;
        color: #fff;

        .arrow-down {

        }
      ;
      }
    }

    .banner {
      height: 151px;
      background: red;
    }
  }

  .content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;

    .steps-container {
      width: 100%;

      .steps-horizontal {
        display: flex;
        white-space: nowrap;

        .step {
          display: inline-block;
          flex-basis: 33%;
          position: relative;
          flex-shrink: 1;

          &.active {
            .step__head {
              .step__line {

              }

              .step__icon {
                -webkit-box-shadow: 0 0 0 4px rgba(0, 0, 0, 0.5);
                box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.5);
                background: #000;
              }
            }

            .step__main {
              .step__title {
                color: #000;

              }
            }
          }

          &:last-child {
            width: 33%;

            .step__head {
              .step__line {
                display: none;
              }
            }
          }

          .step__head {
            text-align: center;
            position: relative;

            .step__line {
              background: #e0e0e0;
              height: 5px;
              position: absolute;
              left: 50%;
              right: -50%;
              top: 11px;
            }

            .step__icon {
              background: #e0e0e0;
              font-size: 18px;
              height: 28px;
              width: 28px;
              color: #ffffff;
              border: 2px solid transparent;
              border-radius: 50%;
              -webkit-box-pack: center;
              -ms-flex-pack: center;
              -webkit-box-align: center;
              -ms-flex-align: center;
              align-items: center;
              box-sizing: border-box;
              display: inline-flex;
              justify-content: center;
              position: relative;
              -webkit-transition: .15s ease-out;
              transition: .15s ease-out;
              z-index: 1;

              .step__icon-inner {
                display: inline-block;
              }
            }
          }

          .step__main {
            display: block;
            text-align: center;
            white-space: normal;

            .step__title {
              color: #e0e0e0;
              font-size: 12px;
              font-weight: 700;
              @media screen and (max-width:505px){
                display:none;
              }
            }
          }
        }
      }

    }

    .form {
      display: flex;
      align-items: center;
      border: 1px solid #ededed;
      padding: 20px 25px;
      max-width: 445px;

      .form-stage {
        width:100%;
        display: flex;
        flex-direction: column;
        gap: 24px;

        justify-content: center;
        margin: 0 auto;

        &:has(.reservation) {
          gap: 12px;
        }

        .inputs-container {
          display: flex;
          gap: 8px 28px;
          flex-wrap: wrap;
          justify-content: center;
          max-width: 388px;
          width:100%;

          @media screen and (max-width:505px){
            flex-direction: column;
          }

          .input-line {
            width: 100%;
            display: flex;
            justify-content: space-between;

            @media screen and (max-width:505px){
              display:flex;
              flex-direction:column;
              gap:8px;
            }

            &:has(.use-promo) {
              margin: 15px 0 15px 5px;
            }

            &:has(.checkbox-container) {
              margin: 5px 0;
            }

            label {
              color: #000;
              font-weight: 400;
              line-height: 34px;
              padding-right: 4px;
              font-size: 14px;
              white-space: nowrap;

              &.stage-two {
                word-wrap: break-word;
                color: #333;
                font-family: NotoSans-Bold;
                font-size: 12px;
                font-weight: 800;
                white-space: normal;
                text-align: left;
                line-height: 19px;
              }
            }

            .checkbox-container {
              display: flex;
              align-items: flex-start;
              gap: 8px;

              * {
                cursor: pointer;
              }
            }

            .input-container {
              display: flex;
              align-items: center;
              gap: 4px;
              width: 180px;
              @media screen and (max-width:505px){
                width:100%;
                &:has(.required){
                  width:calc(100% + 10px);
                }
              }

              &.full-w {
                width: calc(100% + 10px);
              }

              &:has(.required) {
                margin-left: -10px;
              }

              .use-promo {
                border-bottom: 1px solid #333;
                display: inline-block;
                font-size: 12px;
                padding-right: 20px;
                position: relative;
                color: #000;
                cursor: pointer;

                &:before {
                  border-color: #333 transparent;
                  border-style: solid;
                  border-width: 5px 5px 0;
                  content: "";
                  height: 0;
                  position: absolute;
                  right: 0;
                  top: 7px;
                  transition: transform .25s;
                  transition: transform .25s, -webkit-transform .25s;
                  width: 0;
                }

                &:after {
                  border-color: #fff transparent;
                  border-style: solid;
                  border-width: 3px 3px 0;
                  content: "";
                  height: 0;
                  position: absolute;
                  right: 2px;
                  top: 7px;
                  -webkit-transition: all .25s;
                  transition: all .25s;
                  width: 0;
                }
              }

              .required {
                padding-top: 4px;
                color: #f56c6c;
              }

              .input {
                border: 1px solid #d4d4d4;
                border-radius: 4px;
                display: flex;
                padding: 0 8px;
                width: 100%;
                align-items: center;

                &:has(label) {
                  display: flex;
                  align-items: center;
                  gap: 8px;
                  border: none;
                }

                .btn {
                  background: #f1f2f2;
                  border: none;
                  border-radius: 4px;
                  color: #c9cacb;
                  cursor: pointer;
                  display: inline-block;
                  font-size: 14px;
                  font-weight: 400;
                  line-height: 34px;
                  margin-bottom: 0;
                  text-align: center;
                  -webkit-user-select: none;
                  -moz-user-select: none;
                  -ms-user-select: none;
                  user-select: none;
                  vertical-align: middle;
                  white-space: nowrap;
                  width: 90px;

                  &.active {
                    background-color: #10024a;
                    border-color: #10024a;
                    color: #fff;
                  }
                }

                .email-icon {
                  width: 22px;
                  height: 18px;
                }

                .phone-icon {
                  width: 18px;
                  height: 18px;
                }

                .text-input {
                  background: transparent;
                  height: 32px;
                  line-height: 32px;
                  padding: 0 0 0 4px;
                  border: none;
                  color: #000;

                  &.code-input {
                    background-color: #fff;
                    background-image: none;
                    border: 1px solid #ccc;
                    border-radius: 4px;
                    color: #555;
                    font-size: 14px;
                    height: 34px;
                    line-height: 1.42857143;
                    padding: 6px 12px;
                    transition: border-color .15s ease-in-out, box-shadow .15s ease-in-out, -webkit-box-shadow .15s ease-in-out;

                    &:focus {
                      border-color: #333;
                      -webkit-box-shadow: inset 0 1px 1px #000, 0 0 8px rgb(102 175 3.88333333%);
                      box-shadow: inset 0 1px 1px #000, 0 0 8px rgb(102 175 3.88333333%);
                      outline: 0;
                    }
                  }

                  &:focus {
                    outline: none;
                  }
                }
              }
            }

            .textarea {
              background: url(../assets/textareaIcon.png) no-repeat 10px 10px;
              background-size: 14px;
              border: 1px solid #ccc;
              border-radius: 4px;
              height: 75px;
              line-height: 36px;
              padding-left: 35px;
              width: 100%;
              color: #000;

              &:focus {
                outline: none;
              }
            }
          }

          .select-container {
            width: 180px;
            @media screen and (max-width:505px){
              width:100%;
            }
          }

          .floor-container {
            width: 387px;
            @media screen and (max-width:505px){
              width:100%;
            }
          }

        }

        .text-block {
          max-width: 388px;
          color: #000;
          font-size: 12px;
          line-height: 18px;
          display: flex;
          flex-direction: column;
          gap: 16px;
          text-align: left;

          .heading {
            font-weight: 700;
          }
        }

        .terms-agreement {
          display: flex;
          align-items: flex-start;
          justify-content: center;

          * {
            cursor: pointer;
          }

          .checkbox {
            background-color: #FFF;
            border-color: #000;
            display: inline-block;
            margin: 3px 0 0;
            padding: 0;
            width: 20px;
          }

          label {
            font-size: 12px;
            color: #000;
          }
        }

        .button-next {
          background: #f1f2f2;
          border: none;
          border-radius: 4px;
          color: #c9cacb;
          display: block;
          font-family: NotoSans-SemiBold;
          font-size: 12px;
          line-height: 32px;
          margin: 0 auto;
          width: 96%;
          cursor: not-allowed;

          &.allow {
            cursor: pointer;
            color: #fff;
            background: #000;
          }
        }

        .controls {
          display: flex;
          justify-content: space-between;
          margin-top: 20px;

          @media screen and (max-width:505px){
            gap:20px;
            .el-btn{
              width:calc(50% - 20px) !important;
            }
          }

        }

        .terms {
          font-family: Helvetica Neue;
          font-size: 10px;
          font-style: italic;
          text-align: center;
        }

        > .heading {
          background: #fff2bf;
          border-radius: 3px;
          padding: 10px;

          p {
            font-size: 14px;
            margin: 0 auto;
            text-align: center;
          }
        }

        .reservation {
          border: 1px solid #000;
          border-radius: 3px;
          padding: 25px 10px;
          width: 100%;
          text-align: left;

          h4 {
            color: #333;
            font-size: 18px;
            font-weight: 400;
            margin-bottom: 12px;
            margin-left: 10px;

            span {
              color: #1565c0;
            }
          }

          .reservation-data {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-left: 10px;

            .reservation-line {
              display: flex;
              align-items: center;
              gap: 12px;

              span {
                word-wrap: break-word;
                color: #333;
                display: inline-block;
                font-size: 14px;
                width: 80%;
                text-align: left;
                max-width:80%;
                text-overflow: ellipsis;
                white-space: nowrap;
                overflow: hidden
              }

              .reservation-icon {
                width: 20px;
              }
            }

            .cancel-line {
              font-size: 14px;
              display: flex;
              gap: 4px;

              .link {
                color: #1d5bbf;
                cursor: pointer;

                &:hover {
                  text-decoration: underline;
                }
              }

              .text {
                color: #333;
              }
            }
          }
        }

        .sharing {
          .solids {
            border-bottom: 1px solid #d4d4d4;
            height: 8px;
            margin: 0 auto;
            width: 205px;
          }

          .invite {
            font-family: NotoSans-Bold;
            font-size: 12px;
            font-weight: 700;
            line-height: 16px;
            margin-bottom: 10px;
            padding: 8px 0 0;
            text-align: center;
          }

          .input-container {
            display: flex;

            .input {
              border: 1px solid #d4d4d4;
              border-radius: 4px;
              height: 100%;
              width: 100%;
              display: flex;
              align-items: center;
              padding-left: 8px;

              .email-icon {
                width: 17px;
              }

              .text-input {
                background-color: transparent;
                border: none;
                height: 31px;
                line-height: 32px;
                padding: 0 0 0 4px;
                color: #000;
                width: 100%;

                &:focus {
                  outline: none;
                }
              }
            }
          }

          .share-btn {
            background: #f1f2f2;
            border: none;
            border-radius: 4px;
            color: #c9cacb;
            display: block;
            font-family: NotoSans-Bold;
            font-size: 14px;
            font-weight: 700;
            height: 40px;
            margin: 22px auto 15px;
            width: 119px;

            &.active {
              background: #000;
              color: #fff;
            }
          }
          .download-app{
            width:100%;
            .done{
              font-size: 12px;
              font-style: italic;
              margin-top: 10px;
              text-align: center;
              a{
                color:#1565c0;
                font-weight:400;
              }
            }
          }
        }

        .el-btn {
          appearance: none;
          background: #fff;
          border: 1px solid #dcdfe6;
          border-radius: 4px;
          box-sizing: border-box;
          color: #606266;
          cursor: pointer;
          display: inline-block;
          font-size: 14px;
          font-weight: 500;
          line-height: 1;

          &.back-btn {
            background-color: #f7f8fa;
            border: 1px solid #000;
            color: #101125;
            font-family: NotoSans-Regular;
            font-weight: 700;
            height: 36px;
            line-height: 36px;
            padding: 0;
            width: 160px;
          }

          &.confirm-btn {
            background-color: #000;
            border: 1px solid #000;
            color: #fff;
            font-family: NotoSans-SemiBold;
            font-weight: 700;
            height: 36px;
            line-height: 36px;
            padding: 0;
            width: 160px;
          }
        }
      }

    }
    .footer{
      background: #fff;
      font-family: NotoSans-Regular;
      margin: 0 auto;
      text-align: center;
      max-width: 498px;
      width:100%;
      >span{
        color: #2b2a29;
        font-size: 14px;
        line-height: 18px;
        a{
          color: #2b2a29;
          text-decoration: underline;
        }
        i{
          font-style:normal;
        }
        .foot_power{
          text-decoration: none;
          &:hover{
            text-decoration:underline;
          }
          em{
            color: #b5b5b5;
            font-size: 14px;
            font-style: normal;
            line-height: 18px;
          }
        }
      }
    }
  }
}
</style>