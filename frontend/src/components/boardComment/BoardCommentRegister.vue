<template>
  <div>
    <v-card-actions v-if="this.$store.state.isLogin && this.board" class="hidden-xl-only small">

      <v-textarea @focus="setMemberId" solo auto-grow class="commentBox"
                  rows="3" row-height="20" placeholder="댓글 작성" v-model="content">
      </v-textarea>
      <v-btn @click="onClick" depressed color="primary" class="commentBtn">댓글 등록</v-btn>

    </v-card-actions>
    <v-card-actions v-if="this.$store.state.isLogin && this.board" class="hidden-lg-and-down big">

      <v-textarea @focus="setMemberId" solo auto-grow class="commentBox"
                  rows="3" row-height="20" placeholder="댓글 작성" v-model="content">
      </v-textarea>
      <v-btn @click="onClick" depressed color="primary" class="commentBtn">댓글 등록</v-btn>

    </v-card-actions>
  </div>
</template>

<script>
import axios from "axios";
import {mapActions, mapState} from "vuex";

export default {
  name: "BoardCommentRegister",
  props: {
    board: {
      type: Object,
      required: true,
    },
    computed: {
      ...mapState(["session"])
    },
  },
  data() {
    return {
      content: '',
      memberId: 'testId',
      isDeleted: false,
      writer: '',
    }
  },
  methods: {
    ...mapActions(["fetchCommentList"]),
    onClick() {
      const { memberId, content } = this
      const { boardNo } = this.board

      if(content !== '') {
        axios.post(`https://evsbackend.herokuapp.com/comment/register/${boardNo}`, { memberId, content })
            .then(() => {
              alert('댓글 등록 성공!')
              this.fetchCommentList(this.board.boardNo)
              this.content = ""
            })
            .catch(res => {
              //alert(res.response.data.message)
              console.log(res.data)
              alert('댓글 등록 오류')
            })
      }else {
        alert('내용을 입력해 주세요')
      }
    },
    setMemberId() {
      let temp = this.$store.state.session.memberId
      this.memberId = temp
    }
  }
}
</script>

<style scoped>
.small .commentBox{
  position: relative;
  top: 50px;
  width: 50%;
  max-width: 900px;
  margin: auto 0 auto 7%;
  padding: 0 20px;
  box-sizing: border-box;
  border-radius: 5px;
}
.small .commentBtn{
  position: relative;
  top: 35px;
  margin: 0 auto;
  min-height: 50px;
}

.big .commentBox{
  position: relative;
  top: 50px;
  right: 23px;
  padding: 0 380px;
  box-sizing: border-box;
  border-radius: 5px;
}
.big .commentBtn{
  position: relative;
  min-height: 50px;
  top: 35px;
  right: 360px;
}
</style>
