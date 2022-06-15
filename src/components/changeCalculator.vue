<script setup>
import { ref, computed, reactive } from 'vue'
// changeCalculator variable
let productPrice = ref(0)
let customerPay = ref(0)
let change = ref(0)
let typeOfMoneys = [1000, 500, 100, 50, 20, 10, 5, 2, 1];
let typeOfCashs = reactive([0, 0, 0, 0, 0, 0, 0, 0, 0])
let showNotEnoughMoney = ref(false)
let showEqualMoney = ref(false)
let showEnoughMoney = ref(false)

// -- CSS variavle
let changeColorTable = 'background-color: #6d99e4'
let changeColorHas = 'color: red'

// -- Reset ทุกอย่างให้กลับไปเริ่มต้นใหม่
const clearCalculator = () => {
    typeOfCashs.forEach((val,index) => typeOfCashs[index] = 0) // เคลียค่าใน array typeOfCashs  
    change.value = 0 // set เงินทอนให้เป็น 0
    showNotEnoughMoney.value = false
    showEqualMoney.value = false
    showEnoughMoney.value = false
    // console.log(` ${typeOfCashs}`) // debug
}

const calculate = computed(() => {
    clearCalculator()
    change.value = customerPay.value - productPrice.value // คำนวณหาเงินทอน
    let amount = 0;
    let temp = change.value // temp รับค่าเงินทอนมาคำนวณ
    if (temp > 0) {
        for (let i = 0; i < typeOfMoneys.length; i++) {
            if (temp >= typeOfMoneys[i]) {
                amount = Math.floor(temp / typeOfMoneys[i])
                temp = temp - (typeOfMoneys[i] * amount)
                typeOfCashs[i] = amount
            } // for loop เพื่อ set ค่าใน array typeOfCashs เพื่อบอกว่าจะต้องทอนแบงค์ เหรียญอะไรบ้าง
        }
        showEnoughMoney.value = true
        console.log(`ชำระเกินต้องทอน`) // debug
    } else if (temp < 0) {
        showNotEnoughMoney.value = true
        console.log(`เงินไม่พอ`) // debug
    } else {
        showEqualMoney.value = true
        console.log(`ชำระพอดี`) // debug
    }
})
const props = defineProps({
    titleChange: {
        type: String,
        require: true
    }
})
</script>

<template>
    <div class="container">
        <h2 class="title-size-1 title-color"> {{ titleChange }}</h2>
        <div class="container-grid-input">
            <div class="grid-input-item">
                <div>
                    <label for="productprice" class="title-color title-size-2">productPrice</label>
                    <input type="number" name="" id="productprice" v-model="productPrice">
                </div>
                <div>
                    <label for="customerpay" class="title-color title-size-2">customerPay </label>
                    <input type="number" name="" id="customerpay" v-model="customerPay">
                </div>
            </div>
            <div class="grid-input-item">
                <button @click="calculate">Calculate</button>
                <button @click="clearCalculator">Clear</button>
            </div>
        </div>
        <div class="container-grid-show">
            <div>
                <div class=" title-color title-size-2 bold-text">ยอดชำระ</div>
                <span class="title-size-1 bold-text">{{ customerPay > 0 ? customerPay : 0 }}</span>
            </div>
            <div>
                <div class=" title-color title-size-2 bold-text">ราคาสินค้า</div>
                <span class="title-size-1 bold-text">{{ productPrice > 0 ? productPrice : 0 }}</span>
            </div>
            <div>
                <div class=" title-color title-size-2 bold-text">เงินทอน</div>
                <span class="title-size-1 bold-text ">{{ change >= 0 ? change : '-' }}</span>
            </div>
        </div>
        <div>
            <h2 v-if="showEnoughMoney" style="color:green">ขอบคุณที่ใช้บริการ</h2>
            <h2 v-else-if="showNotEnoughMoney" style="color:red">ยอดชำระไม่เพียงพอ <br> จำนวนเงินที่ขาด {{ change * -1
            }}
                บาท</h2>
            <h2 v-else-if="showEqualMoney" style="color:green">ยอดชำระพอดี</h2>
        </div>
        <div class="container-change">
            <h2 class="title-color title-size-2">ประเภทธนบัตร/เหรียญ</h2>
            <div class="container-grid-topic-change">
                <h2>ประเภท</h2>
                <h2></h2>
                <h2>หน่วย</h2>
                <h2>จำนวน</h2>
                <h2>หน่วย</h2>
            </div>
            <div v-for="(typeOfMoney, index) in typeOfMoneys" :key="index" class="container-grid-details-change"
                :style="index % 2 == 1 ? '' : changeColorTable">
                <div :style="typeOfCashs[index] != 0 ? changeColorHas : ''">{{ typeOfMoney > 10 ? 'ธนบัตร' : 'เหรียญ' }} </div>
                <div :style="typeOfCashs[index] != 0 ? changeColorHas : ''">{{ typeOfMoney }}</div>
                <div :style="typeOfCashs[index] != 0 ? changeColorHas : ''">บาท</div>
                <div :style="typeOfCashs[index] != 0 ? changeColorHas : ''">{{ typeOfCashs[index] }}</div>
                <div :style="typeOfCashs[index] != 0 ? changeColorHas : ''">{{ typeOfMoney > 10 ? 'ใบ' : 'เหรียญ' }}</div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.title-size-1 {
    font-size: 2.1vw;
}

.title-size-2 {
    font-size: 1.6vw;
}

.title-size-3 {
    font-size: 1.1vw;
}

.title-color {
    color: #2346a6;
}

.bold-text {
    font-weight: bold;
}

/* -------------------------- */

.container {
    display: block;
    max-width: 80%;
    margin: auto;
    background-color: #a2c8ff;
    padding: 20px;
    text-align: center;
}

/* -------------------- */
.container-grid-input {
    display: grid;
    grid-template-columns: auto auto;
}

.grid-input-item {
    display: flex;
    flex-direction: column;
    justify-content: space-between
}

.grid-input-item label {
    font-weight: bold;
    margin-right: 25px;
}

.grid-input-item input[type=number] {
    width: 50%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}

.grid-input-item button {
    background-color: #3660b9;
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: block;
    font-size: 2vw;
    margin: 4px 2px;
    cursor: pointer;
    width: 80%;
}

/* -------------------- */

.container-change {
    border: 1px solid black;
}

.container-change h2 {
    margin: 10px 0;
}

.container-grid-show {
    display: grid;
    grid-template-columns: auto auto auto;
    padding: 10px 0;
}

/* -------------------- */
.container-grid-topic-change {
    display: grid;
    grid-template-columns: 20% 20% 20% 20% 20%;
    background-color: #3660b9;
    text-align: end;
    padding: 0 5%;
    font-size: 1.3vw;
}

.container-grid-details-change {
    display: grid;
    grid-template-columns: 20% 20% 20% 20% 20%;
    text-align: end;
      padding: 5px 5%;
}

.container-grid-details-change>div {
    font-size: 1.3vw;
}

/* -------------------- */
</style>
