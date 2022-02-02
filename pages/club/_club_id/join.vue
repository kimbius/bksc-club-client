<template>
  <div class="mt-5">
    <v-container>
      <v-row justify="center" align="center">
        <v-col cols="12" sm="8">
          <v-card
            :loading="loading"
          >
            <v-card-title>เข้าร่วมชุมนุม {{ club_detail.name }} </v-card-title>
            <v-card-text>
              <ClubChip :club="club_detail" />
              <v-btn
                :to="'/club/' + club_detail.id"
                small
                depressed
                color="grey darken-3"
                class="white--text mt-2"
              >
                <b-icon
                  icon="people"
                  class="v-icon notranslate v-icon--left"
                ></b-icon>
                รายชื่อสมาชิก
              </v-btn>
            </v-card-text>
            <template v-if="club_detail.memberCount < club_detail.maxMember">
              <v-stepper v-model="e6" vertical>
                <v-stepper-step :complete="e6 > 1" step="1">
                  ยืนยันหมายเลขประจำตัวนักเรียน
                  <small>กรอกหมายเลขประจำตัวนักเรียน</small>
                </v-stepper-step>
                <v-stepper-content step="1">
                  <v-otp-input
                    length="5"
                    @input="handleOtpChanged"
                  ></v-otp-input>
                  <v-btn
                    color="primary"
                    :loading="loading_btn"
                    @click="check_studentnumber()"
                  >
                    <b-icon
                      icon="search"
                      class="v-icon notranslate v-icon--left"
                    ></b-icon>
                    ตรวจสอบ
                  </v-btn>
                  <v-btn to="/" depressed color="red"> ยกเลิก </v-btn>
                </v-stepper-content>

                <v-stepper-step :complete="e6 > 2" step="2">
                  ยืนยันการเข้าร่วมชุมนุม
                </v-stepper-step>

                <v-stepper-content step="2">
                  <div class="mb-5" v-if="student_detail.name">
                    <h2>สวัสดี {{ student_detail.name }}</h2>
                    <h3 v-if="student_detail.club">
                      ชุมนุมของคุณ :
                      <span class="primary--text text--lighten-3">{{
                        student_detail.club
                      }}</span>
                    </h3>
                    <h3 v-else>
                      หากคุณยืนยันคุณจะไม่สามารถเปลี่ยนแปลงในครั้งต่อไปได้
                    </h3>
                  </div>
                  <div v-else></div>
                  <v-btn
                    :disabled="!student_detail.name || student_detail.club"
                    color="green"
                    :loading="loading_btn"
                    @click="join_club()"
                  >
                    <b-icon
                      icon="door-open"
                      class="v-icon notranslate v-icon--left"
                    ></b-icon>
                    {{
                      student_detail.club
                        ? "คุณมีชุมนุมอยู่แล้ว"
                        : !student_detail.name
                        ? "ไม่พบนักเรียน"
                        : "ยืนยันที่จะเข้าร่วม"
                    }}
                  </v-btn>
                  <v-btn depressed color="red" @click="e6 = 1"> กลับ </v-btn>
                </v-stepper-content>

                <v-stepper-step :complete="e6 > 3" step="3">
                  เข้าร่วมชุมนุมสำเร็จ
                </v-stepper-step>

                <v-stepper-content step="3">
                  <div class="mb-5">
                    <h1>🎉 ยินดีด้วย!</h1>
                    <h3>คุณเข้าร่วมชุมนุมนี้สำเร็จแล้ว</h3>
                  </div>
                  <v-btn to="/" depressed color="primary">
                    เยี่ยมชมชุมนุมอื่นๆ
                  </v-btn>
                </v-stepper-content>
              </v-stepper>
            </template>
            <template v-else>
              <v-card-text class="text-center">
                <h3>ปิดรับสมัครแล้ว</h3>
              </v-card-text>
            </template>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-snackbar v-model="snackbar_data.open" :timeout="2000">
      {{ snackbar_data.text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="snackbar_data.open = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  data() {
    return {
      e6: 1,
      student_number: 0,
      student_detail: {},
      club_id: this.$route.params.club_id,
      club_detail: {},
      loading: true,
      loading_btn: false,
      snackbar_data: {
        open: false,
        text: "",
      },
    };
  },
  mounted() {
    this.$axios.$get("/clubs/" + this.club_id + "/info").then((res) => {
      this.club_detail = res;
      this.loading = false;
    });
  },
  methods: {
    handleOtpChanged(value) {
      this.student_number = value;
    },
    check_studentnumber() {
      this.loading_btn = true;
      this.$axios
        .$get("/clubs/getstudent?student_id=" + this.student_number)
        .then((res) => {
          this.loading_btn = false;
          this.e6 = 2;
          this.student_detail = res;
          this.snackbar("สวัสดี " + this.student_detail.name, "");
        })
        .catch((err) => {
          this.loading_btn = false;
          if (err.response.status == 404)
            return this.snackbar("ไม่พบนักเรียน", "");
          this.snackbar("มีข้อผิดพลาดเกิดขึ้น", "");
        });
    },
    join_club() {
      this.loading_btn = true;
      this.$axios
        .$post("/clubs/" + this.club_id + "/join", {
          student_id: this.student_number,
        })
        .then((res) => {
          this.loading_btn = false;
          this.e6 = 3;
          this.snackbar("เข้าร่วมชุมนุมแล้ว", "");
        })
        .catch((err) => {
          this.loading_btn = false;
          this.snackbar("มีข้อผิดพลาดเกิดขึ้น", "");
        });
    },
    snackbar(text, type) {
      this.snackbar_data.open = true;
      this.snackbar_data.text = text;
    },
  },
};
</script>