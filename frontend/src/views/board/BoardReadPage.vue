<template>
  <div class="comment-container">
    <v-container class="pa-0">
      <board-read-page-form v-if="board" :board="board"/>

      <!-- 댓글 등록 -->
      <board-comment-register v-if="board" :board="board"></board-comment-register>

      <!-- 댓글 리스트 -->
      <board-comment-list v-if="boardNo" :board-no="boardNo"></board-comment-list>
    </v-container>
  </div>
<!--  <v-container>-->
<!--    <div>-->
<!--      <board-read-page-fom v-if="board" :board="board"/>-->

<!--      &lt;!&ndash; 댓글 &ndash;&gt;-->
<!--      <board-comment-register></board-comment-register>-->
<!--    </div>-->
<!--  </v-container>-->

</template>

<script>
import { mapActions, mapState } from 'vuex'
import BoardReadPageForm from '../../components/board/BoardReadPageForm.vue'
import BoardCommentRegister from "../../components/boardComment/BoardCommentRegister";
import BoardCommentList from "../../components/boardComment/BoardCommentList";
export default {
  components: {BoardCommentList, BoardCommentRegister, BoardReadPageForm },
    name: 'BoardReadPage',
    props: {
        boardNo: {
            required: true
        },
        },
        computed:{
            ...mapState(['board' ])
        },
        methods: {
            ...mapActions(['fetchBoard'])
        },
        created() {
            console.log(this.boardNo)
            this.fetchBoard(this.boardNo)
        },
    }

</script>
