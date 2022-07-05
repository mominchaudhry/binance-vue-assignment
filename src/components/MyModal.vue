<script>
export default {
  props: {
    show: Boolean,
    item: Object
  },
  methods: {
    setContent(e){ this.content = e.target.value },
    setGroup(e){ this.group = e.target.value },
    setStatus(e){ this.status = e.target.value },
    setType(e){ this.type = e.target.value },
    setTime(e){ this.sendTime = e.target.value },
    setCreator(e){ this.creator = e.target.value },
    close() {
      this.$emit("close")
      this.content = ""
      this.group = ""
      this.status = ""
      this.type = ""
      this.sendTime = ""
      this.creator = ""
    },
    getTime() {
      const today = new Date();

      let month = today.getMonth()+1
      if (month < 10) month = "0" + month
      let day = today.getDate()
      if (day < 10) day = "0" + day

      const date = today.getFullYear()+'-'+(month)+'-'+day;

      let minute = today.getMinutes()
      if (minute < 10) minute = "0" + minute
      let second = today.getSeconds()
      if (second < 10) second = "0" + second

      const time = today.getHours() + ":" + minute + ":" + second;
      const dateTime = date +' '+ time;

      return dateTime;
    },
    add() {
      this.$emit('add-item', {
        id: this.id++,
        content: this.content,
        group: this.group,
        status: this.status,
        type: this.type,
        time: this.sendTime,
        createTime: this.createTime || this.getTime(),
        creator: this.creator
      })
      this.close()
    }
  },
  updated() {
    if(this.item) {
      this.id = this.item.id
      this.content = this.item.content
      this.group = this.item.group
      this.status = this.item.status
      this.type = this.item.type
      this.sendTime = this.item.time
      this.createTime = this.item.createTime
      this.creator = this.item.creator
    }
  },
  data() {
    return {
      content: this.item?.content || "",
      group: this.item?.group || "",
      status: this.item?.status || "",
      type: this.item?.type || "",
      sendTime: this.item?.time || "",
      creator: this.item?.creator || "",
      id: this.item?.id || localStorage.getItem("id") || 0
    }
  }
}
</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header">Create Message</slot>
          </div>

          <div class="modal-body">
            <slot name="body">
              <div class="input">Content: <input :value="content" @input="setContent"></div>
              <div class="input">Group: <input :value="group" @input="setGroup"></div>
              <div class="input">Status: <input :value="status" @input="setStatus"></div>
              <div class="input">Type: <input :value="type" @input="setType"></div>
              <div class="input">Send Time: <input :value="sendTime" @input="setTime"></div>
              <div class="input">Creator: <input :value="creator" @input="setCreator"></div>
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <button
                class="modal-default-button"
                @click="close"
              >Cancel</button>
              <button
                class="modal-default-button"
                @click="add"
              >Create/Update</button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.input {
  padding: 5px;
}
</style>