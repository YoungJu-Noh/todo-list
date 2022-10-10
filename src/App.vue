<template>
  <div id="container">
    <span></span>

    <h1 id="hstyle">
      <img
        src="./views/study_icon.png"
        alt="study image"
        width="35"
        height="40"
      />
      STUDY ToDo LIST
    </h1>

    <span v-if="isModify === false">
      <p>
        <label for="subject">Choose a subject : </label>
        <select v-model="select">
          <option v-bind:value="kor">{{ kor }}</option>
          <option v-bind:value="eng">{{ eng }}</option>
          <option v-bind:value="math">{{ math }}</option>
          <option v-bind:value="ex">{{ ex }}</option>
        </select>
        &nbsp;&nbsp;

        <input
          class="memo"
          type="text"
          v-model="comment"
          placeholder="To do list를 작성해주세요."
          ref="input_ref"
        />
        <span class="submit" @click="apply">등록</span>
      </p>
    </span>

    <span v-else>
      <label for="subject">Choose a subject : </label>
      <select v-model="selectModify">
        <option v-bind:value="kor">{{ kor }}</option>
        <option v-bind:value="eng">{{ eng }}</option>
        <option v-bind:value="math">{{ math }}</option>
        <option v-bind:value="ex">{{ ex }}</option>
      </select>
      &nbsp;&nbsp;

      <input
        name="inputModify"
        class="memoModify"
        type="text"
        v-model="commentModify"
        placeholder="수정할 내용을 이곳에 작성해주세요"
        ref="input_refModify"
      />
      <span class="submitModify" @click="applyModify">수정</span>
    </span>

    <ul class="todo-ul">
      <li class="todo-li" v-for="(todo, index) in todoList" :key="index">
        <span
          style="color: red"
          v-show="todo.createdAt.getTime() + 1000 * 10 > new Date().getTime()"
          >New</span
        >

        <input
          type="checkbox"
          v-model="checkboxList[index]"
          :value="index"
          @change="checkChange(index)"
        />
        <span v-if="this.checkboxList[index] === true">
          <del>{{ todo.commentItem }}</del></span
        >
        <span v-else> {{ todo.commentItem }}</span>

        <img
          @click="deleteTodo(index)"
          class="icon"
          align="right"
          vertical-align="middle"
          src="./assets/delete_icon.png"
        />
        <img
          @click="modifyTodo(index)"
          class="icon"
          align="right"
          vertical-align="middle"
          src="./assets/modify_icon.png"
        />
      </li>

      <li class="todo-li" v-if="todoList.length === 0">List</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todoList: [],
      checkboxList: [],

      comment: "",
      commentModify: "",
      isModify: false,
      indexModify: 0,

      selectModify: "국어",
      select: "국어",
      kor: "국어",
      eng: "영어",
      math: "수학",
      ex: "기타",
    };
  },

  methods: {
    checkChange(index) {
      console.log("index : ", index);
    },

    apply() {
      if (!this.comment) {
        alert("To do List를 작성해주세요.");
        this.$refs.input_ref.focus();
        return;
      }

      this.todoList.push({
        commentItem: this.select + " - " + this.comment,
        createdAt: new Date(),
      });

      this.comment = "";
    },

    applyModify() {
      if (!this.commentModify) {
        alert("수정내용을 작성해주세요.");
        this.$refs.input_refModify.focus();
        return;
      }

      console.log("index : ", this.indexModify);
      console.log("commentModify : ", this.commentModify);

      this.todoList[this.indexModify].commentItem =
        this.selectModify + " - " + this.commentModify;

      this.commentModify = "";
      this.isModify = false;
    },

    deleteTodo(index) {
      this.todoList.splice(index, 1);
    },

    modifyTodo(index) {
      console.log("index : ", index);
      this.isModify = true;
      this.indexModify = index;
    },
  },

  created() {
    setInterval(() => {
      this.todoList = this.todoList.map((todo) => todo);
    }, 2000);
  },
};
</script>

<style scoped>
#hstyle {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 50px;
}
.memo {
  width: 400px;
  height: 50px;
  margin: 10px auto;
}
.memoModify {
  width: 400px;
  height: 50px;
  margin: 10px auto;
}
#container {
  border: 2px solid #000000;
  border-style: double;
  width: 900px;
  height: 700px;
  margin: 50px auto;
  text-align: center;
  box-shadow: 0 20px 15px lightgray;
  overflow: auto;
  background-image: url("./views/study_icon1.png");
  background-size: 40px, 50px;
  background-repeat: no-repeat;
  background-position: bottom right;
}

.submit {
  margin-left: 10px;
  border: 1px solid rgb(34, 137, 205, 0.745);
  background-color: rgba(34, 137, 205, 0.745);
  width: 60px;
  display: inline-block;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 2px 5px #000000;
  font-style: #ffffff;
}
.submitModify {
  margin-left: 10px;
  border: 1px solid rgba(255, 118, 80, 0.934);
  background-color: rgba(255, 118, 80, 0.934);
  width: 60px;
  display: inline-block;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 2px 5px #000000;
  font-style: #ffffff;
}

.todo-li {
  width: 750px;
  height: 40px;
  list-style: square;
  border-bottom-style: dotted;
  border-color: lightblue;
  text-align: left;
  vertical-align: middle;
  box-align: center;
  margin: 30px;
}

.icon {
  width: 20px;
  height: 20px;
  vertical-align: baseline;
  box-align: right;
  padding: 15px;
}
</style>
