<template>
  <div class="container">
    <p>Count is: {{ count }}</p>
    <div class="button-group">
      <button type="button" class="btn btn-primary" @click="confirmAction('increment')">
        Increment
      </button>
      <button type="button" class="btn btn-primary" @click="confirmAction('reduce')">
        Reduce
      </button>
      <button type="button" class="btn btn-primary" @click="confirmAction('reset')">
        Reset
      </button>
      <button type="button" class="btn btn-primary" @click="confirmAction('test')">
        empty
      </button>
    </div>
    <!-- <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Hello</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            {{ modalText }}
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">取消</button>
            <button type="button" class="btn btn-primary" @click="confirmAction" data-bs-dismiss="modal">確定</button>
          </div>
        </div>
      </div>
    </div> -->
    <div class="point">
      <svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
        <symbol id="check-circle-fill" fill="currentColor" viewBox="0 0 16 16">
          <path
            d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zm-3.97-3.03a.75.75 0 0 0-1.08.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-.01-1.05z" />
        </symbol>
        <symbol id="info-fill" fill="currentColor" viewBox="0 0 16 16">
          <path
            d="M8 16A8 8 0 1 0 8 0a8 8 0 0 0 0 16zm.93-9.412-1 4.705c-.07.34.029.533.304.533.194 0 .487-.07.686-.246l-.088.416c-.287.346-.92.598-1.465.598-.703 0-1.002-.422-.808-1.319l.738-3.468c.064-.293.006-.399-.287-.47l-.451-.081.082-.381 2.29-.287zM8 5.5a1 1 0 1 1 0-2 1 1 0 0 1 0 2z" />
        </symbol>
        <symbol id="exclamation-triangle-fill" fill="currentColor" viewBox="0 0 16 16">
          <path
            d="M8.982 1.566a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566zM8 5c.535 0 .954.462.9.995l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995A.905.905 0 0 1 8 5zm.002 6a1 1 0 1 1 0 2 1 1 0 0 1 0-2z" />
        </symbol>
      </svg>

      <div v-if="alertStatus" class="alert alert-primary d-flex align-items-center" role="alert">
        <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Info:">
          <use xlink:href="#info-fill" />
        </svg>
        <div>
          {{ alertMessage }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
      // modalText: '',
      type: '',
      alertMessage: '',
      alertStatus: false
    }
  },
  methods: {
    /**buttontext(action) {
      this.type = action;
      if (action === 'increment') {
        this.modalText = '確定要增加1秒嗎';
      } else if (action === 'reduce') {
        this.modalText = '確定要減少1秒嗎';
      } else if (action === 'reset') {
        this.modalText = '確定要重置嗎';
      }
    },**/
    confirmAction(type) {
      this.type = type;
      if (this.type === 'increment') {
        this.count++;
        this.showAlert('增加count次數=' + this.count);
      } else if (this.type === 'reduce') {
        if (this.count > 0) {
          this.count--;
          this.showAlert('減少count次數=' + this.count);
        }
      } else if (this.type === 'reset') {
        this.count = 0;
        this.showAlert('重置count次數');
      }else {
        this.count = 0;
        this.showAlert('');
      }
    },
    //透過alertmessage是否有內容來判斷要不要呈現alert(alertstatus)
    //使用v-if 去判斷 alertstatus如果是true要呈現 alert,反之要隱藏alert
    showAlert(message) {
      if (!!message) {  //如果message有値的話會做裡面的事情  !=是不等於
        this.alertMessage = message;

        this.alertStatus = true;//狀態是true才做alert , confirmAction()的事情
        //過了3秒隱藏
        setTimeout(() => {
          this.alertStatus = false;  
        }, 3000);
      }
     

    }

  }
}


</script>

<style>
p {
  font-size: 20px;
  padding-top: 10px;
}

.container {
  border: 1px solid black;
  border-radius: 5px;

}


.btn {
  margin: 20px;

}

.button-group .btn-primary {
  font-size: 20px;
  font-weight: bold;
}

.point {
  position: fixed;
  top: 10px;
  right: 10px;


}
</style>