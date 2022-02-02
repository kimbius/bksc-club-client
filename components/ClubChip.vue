<template>
  <div>
    <v-chip
      small
      :color="
        club.memberCount >= club.maxMember ? 'red lighten-1' : 'green accent-4'
      "
      text-color="white"
      >{{ club.memberCount }} / {{ club.maxMember }}</v-chip
    >
    <v-chip
      :color="club.permission == 'EVERYONE' ? 'primary' : 'red'"
      text-color="white"
      small
      >{{
        club.permission == "EVERYONE"
          ? "ทุกระดับชั้น"
          : club.permission == "ONLY_JUNIOR"
          ? "เฉพาะนักเรียนชั้น ม.1 - ม.3"
          : "เฉพาะนักเรียนชั้น ม.4 - ม.6"
      }}</v-chip
    >
  </div>
</template>

<script>
export default {
  props: ["club"],
};
</script>