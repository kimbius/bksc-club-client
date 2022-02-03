<template>
  <div>
    <div class="my-5">
      <v-container>
        <h2 class="text-center">
          ขั้นตอนและวิธีการเข้าร่วมชุมนุมด้วยเว็บไซต์ bksc.ac.th/club
        </h2>
        <v-row justify="center" align="center">
          <v-col cols="12" sm="8">
            <v-timeline align-top :dense="$vuetify.breakpoint.smAndDown">
              <v-timeline-item
                v-for="(item, i) in items"
                :key="i"
                :icon="item.icon"
                fill-dot
              >
                <v-card :color="item.color">
                  <v-card-title v-text="item.title"> </v-card-title>
                  <v-card-text>{{ item.text || item.title }}</v-card-text>
                </v-card>
              </v-timeline-item>
            </v-timeline>
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
      items: [
        {
          title: "เข้าสู่เว็บไซต์ bksc.ac.th/club",
          icon: "mdi-open-in-app",
        },
        {
          title: "ตรวจสอบรายชื่อชุมนุมที่ต้องการสมัคร",
          icon: "mdi-magnify",
          text: "นักเรียนสามารถค้าหาชุมนุมได้โดยกรอกบางคำจากชื่อชุมนุม ชื่อ หรือสกุล ครูผู้สอนจากช่องค้นหา ข้อมูลจะแสดงชุมนุมที่มีข้อความเกี่ยวข้องกับชื่อชุมนุมหรือชื่อหรือนามสกุลกับคำค้นหาที่กรอกลงไป",
        },
        {
          title: "เมื่อพบชุมนุมที่ต้องการสมัคร",
          icon: "mdi-format-list-text",
          text: 'ให้นักเรียนอ่านรายละเอียดของชุมนุมดังกล่าว ว่ารับสมัครนักเรียนระดับชั้นใด จำนวนผู้สมัครครบตามจำนวนที่รับได้หรือยัง เมื่ออ่านรายละเอียดครบแล้วให้กดที่ "คลิ๊กเพื่อสมัคร"',
        },
        {
          title: "กรอกเลขประจำตัวนักเรียน 5 หลัก",
          icon: "mdi-account-multiple-check",
          text: '(ถ้าไม่ทราบเลขประจำตัวใ้ห้สอบถามยังครูที่ปรึกษา) จากนั้นกดปุ่ม "คลิ๊กเพื่อไปยังขั้นตอนถัดไป"',
        },
        {
          title: "ตรวจสอบและยืนยันการเข้าร่วมชุมนุม",
          icon: "mdi-card-account-details",
          text: 'ในหน้านี้จะแสดงผล ชุมนุมที่นักเรียนเลือกพร้อมรายละเอียดของชุมนุม และข้อมูลส่วนตัวของนักเรียน ให้นักเรียนตรวจสอบความถูกต้อง เมื่อตรวจสอบแล้วว่าข้อมูลถูกต้องให้กดปุ่ม "ยืนยันที่จะเข้าร่วม"',
        },
        {
          title: "เข้าร่วมชุมนุมสำเร็จ",
          text: 'เมื่อขึ้นข้อความ "คุณเข้าร่วมชุมนุมนี้สำเร็จแล้ว" ถือเป็นอันเสร็จสิ้น!',
          icon: "mdi-text-box-check",
        },
      ],
    };
  },
};
</script>