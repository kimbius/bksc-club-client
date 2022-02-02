<template>
  <div class="mt-5">
    <v-container>
      <v-row justify="center" align="center">
        <v-col cols="12" sm="8">
          <v-card>
            <v-card-text class="pb-0">
              <v-alert
                outlined
                type="warning"
                icon="mdi-school"
                prominent
                border="left"
              >
                เว็บไซต์นี้ไม่ใช่เว็บไซต์ Official ของทางโรงเรียน
                และเราไม่มีเจตนาร้ายแต่อย่างใด ได้โปรดอย่าคิดไปเอง
                ถ้าไม่มั่นใจอย่าหาเข้าเว็บไซต์
                <br />
                วิธีการใช้งานและหลักการของการทำงานของเว็บไซต์เหมือนกันทุกประการยกเว้นหน้าตาของเว็บไซต์ที่เปลี่ยนไป
                <br/>
                และหากมีข้อผิดพลาดใดๆ เราจะไม่รับผิดชอบใดๆทั้งสิ้น!
              </v-alert>
              <v-text-field
                label="ค้นหาชุมนุมจากชื่อชุมนุม หรือชื่อ หรือนามสกุลครู"
                filled
                dense
                v-model="club_query"
                @input="update_club"
              ></v-text-field>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12" sm="8">
          <v-pagination
          class="mb-4"
            v-model="currentPage"
            :length="Math.ceil(clubs_forshow.length / 5)"
            @input="handlePageChange"
            total-visible="7"
          ></v-pagination>
          <v-card class="mb-3" v-for="(club, i) in paginate(clubs_forshow, 5, currentPage)" :key="i">
            <v-card-title
              ><h3>ชุมนุม : {{ club.name }}</h3></v-card-title
            >
            <v-card-text>
              <ClubChip :club="club" />
              <h3 class="pt-1">
                ครูผู้สอน:
                <span class="primary--text text--lighten-3">{{
                  club.teacher
                }}</span>
              </h3>
            </v-card-text>
            <v-card-actions class="pt-0">
              <v-btn
                :to="'/club/' + club.id + '/join'"
                :disabled="club.memberCount >= club.maxMember"
                large
                depressed
                color="green"
                class="white--text"
              >
                <b-icon
                  icon="door-open-fill"
                  class="v-icon notranslate v-icon--left"
                ></b-icon>
                {{
                  club.memberCount >= club.maxMember ? "ปิดรับสมัคร" : "เข้าร่วมชุมนุม"
                }}
              </v-btn>
              <v-btn
                :to="'/club/' + club.id"
                large
                depressed
                color="grey darken-3"
                class="white--text"
              >
                <b-icon
                  icon="search"
                  class="v-icon notranslate v-icon--left"
                ></b-icon>
                รายชื่อสมาชิก
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filter_select: "",
      filter_items: ["ทุกระดับชั้น", "เฉพาะ ม.ต้น", "เฉพาะ ม.ปลาย"],
      club_query: "",
      clubs_forshow: [],
      clubs: [],
      currentPage: 1,
      pageLength: 1,
    };
  },
  async mounted() {
    this.clubs = await this.$axios.$get("/clubs");
    this.clubs_forshow = this.clubs;
    this.pageLength = Math.floor(this.clubs_forshow.length / 6);
  },
  methods: {
    handlePageChange(value) {
      this.currentPage = value;
    },
    update_club() {
      this.clubs_forshow = this.clubs.filter(
        (_) =>
          _.name.toLowerCase().includes(this.club_query.toLowerCase()) ||
          _.teacher.toLowerCase().includes(this.club_query.toLowerCase())
      );
      this.currentPage = 1
    },
    paginate(array, page_size, page_number) {
      return array.slice(
        (page_number - 1) * page_size,
        page_number * page_size
      );
    },
  },
};
</script>