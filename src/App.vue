<template>
  <div class="main">
    <FortuneWheel
      ref="child"
      @prizesRandomIndex="prizeIndex"
      @onCanvas="onCanvasRotateStart"
      @onRotateEnd="onRotateEnd"
      v-if="allSpinRewards.length"
      :prizes="allSpinRewards"
      :prizesId="0"
      :soundStatus="true"
      :imgBorder="true"
      :styleWheel="styleWheel"
      :happyRain="true"
    />

    <ModalDialog
      @visible="visible"
      :statusVisible="showModal"
      :title="modalData.title"
      :message="modalData.message"
      :discountCode="modalData.discountCode"
    />

    <button class="StartBtn" @click="onCanvasRotateStart">SPIN</button>
  </div>
</template>

<script>
import { ref, reactive, onMounted } from "vue";
import FortuneWheel from "./components/FortuneWheel.vue";
import ModalDialog from "./components/modalDialog.vue";

export default {
  components: {
    FortuneWheel,
    ModalDialog,
  },
  setup() {
    const child = ref(null);
    const showModal = ref(false);
    const randomId = ref(null);
    
    const modalData = reactive({
      title: "",
      message: "",
      discountCode: "",
    });
    
    const styleWheel = reactive({
      colorText: "black",
      twoLineText: {
        status: false,
        outlineColor: "#000",
        inlineColor: "#fff",
        lineWidth: 3,
      },
      font: "bold 16px Arial",
      textAlign: "center",
      textBaseline: "middle",
      textOrientation: "vertical",
      shadow: {
        shadowColor: "rgba(0, 0, 0, 0.5)",
        shadowBlur: 14,
        shadowOffsetX: 0,
        shadowOffsetY: 0,
      },
    });
    
    const allSpinRewards = reactive([
      { id: 0, name: "Nothing", value: 5, probability: 30, bgColor: "#00afad" },
      { id: 1, name: "100 Points", value: 5, probability: 20, bgColor: "#fff" },
      { id: 2, name: "10 Voucher", value: 5, probability: 15, bgColor: "#f173ac" },
      { id: 3, name: "$3 Wallet", value: 5, probability: 10, bgColor: "#00afad" },
      { id: 4, name: "30% Discount", value: 5, probability: 10, codePesentCopon: "zghruigh", bgColor: "#fff" },
      { id: 5, name: "$3 Discount", value: 5, probability: 10, bgColor: "#f173ac" },
    ]);
    
    const onCanvasRotateStart = async () => {
      if (child.value) {
        child.value.spinWheel();
      }
    };

    const onRotateEnd = () => {
      console.log(randomId.value, "selectedPrize");
      if (randomId.value.name === "Nothing") {
        modalData.title = "😞 Sorry!";
        modalData.message = "Unfortunately, you didn’t win anything this time, but try your luck again! 🎡";
        modalData.discountCode = "";
      } else {
        modalData.title = "🎉 Congratulations! 🎉";
        modalData.message = `You won ${randomId.value.name}! 🎁`;
        modalData.discountCode = randomId.value.codePesentCopon || "";
      }
      showModal.value = true;
    };

    const prizeIndex = (index) => {
      console.log(index, "prizesRandomIndex : ");
      randomId.value = index;
    };

    const visible = (toggle) => {
      showModal.value = toggle;
    };

    return {
      child,
      showModal,
      randomId,
      modalData,
      styleWheel,
      allSpinRewards,
      onCanvasRotateStart,
      onRotateEnd,
      prizeIndex,
      visible,
    };
  },
};
</script>

<style>
.StartBtn {
  padding: 10px;
  width: 100px;
  cursor: pointer;
  background-color: pink;
}
.main {
  width: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 30px;
  justify-content: center;
  height: 100vh;
}
</style>