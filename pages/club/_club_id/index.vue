<template>
  <div>
    <div class="mt-5">
      <v-container>
        <v-row justify="center" align="center">
          <v-col cols="12" sm="8">
            <v-card :loading="loading">
              <v-card-title>ชุมนุม : {{ club_detail.name }} </v-card-title>
              <v-card-text>
                <ClubChip :club="club_detail" />
                <v-btn
                  :to="'/club/' + club_detail.id + '/join'"
                  :disabled="club_detail.memberCount >= club_detail.maxMember"
                  small
                  depressed
                  color="grey darken-3"
                  class="white--text mt-2"
                >
                  <b-icon
                    icon="door-open"
                    class="v-icon notranslate v-icon--left"
                  ></b-icon>

                  {{
                    club_detail.memberCount >= club_detail.maxMember
                      ? "ปิดรับสมัคร"
                      : "เข้าร่วมชุมนุม"
                  }}
                </v-btn>
                <h3 class="pt-3">
                  ครูผู้สอน:
                  <span class="primary--text text--lighten-3">{{
                    club_detail.teacher
                  }}</span>
                </h3>

                <v-card>
                  <v-simple-table class="mt-4" dense>
                    <template v-slot:default>
                      <thead>
                        <tr>
                          <th class="text-left">ลำดับ</th>
                          <th class="text-left">ชื่อ-นามสกุล</th>
                          <th class="text-left">ห้อง</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="(item, i) in club_detail.members" :key="i">
                          <td>{{ i + 1 }}</td>
                          <td><b-icon :class="(item.gender =='female' ? 'pink' : 'blue')+'--text'" icon="person-fill"></b-icon> {{ item.name }}</td>
                          <td>{{ item.room }}</td>
                        </tr>
                      </tbody>
                    </template>
                  </v-simple-table>
                </v-card>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
      club_id: this.$route.params.club_id,
      club_detail: {},
    };
  },
  async mounted() {
    const res = await this.$axios.$get("/clubs/" + this.club_id + "/info");
    this.club_detail = res;
    this.loading = false;
  },
};
</script>