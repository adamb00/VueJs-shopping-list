<template class="main">
   <div class="input">
      <div class="inputFields">
         <input type="number" v-model="amount" class="amount" min="1" />
         <select v-model="unit" class="unit">
            <option value="-1">Choose one</option>
            <option>KG</option>
            <option>L</option>
            <option>DB</option>
            <option>DKG</option>
         </select>
         <input type="text" v-model="item" class="item" />
      </div>

      <button class="btnAdd btn" :disabled="!item || !unit || unit < 0 ? true : false" @click="addItem()">Add</button>
   </div>
   <div class="display">
      <div ref="showItem()">{{ showItem() }}</div>
      <div>
         <div v-if="!sortItems()">
            <div v-for="item in itemList">
               <p v-if="item" class="elem">{{ item }}<ion-icon name="trash-bin-outline" class="binIcon" @click="deleteItem(item)"></ion-icon></p>
            </div>
         </div>
         <div v-else-if="sortItems()">
            <div v-for="item in sortedItemList">
               <p v-if="item" class="elem">{{ item }}<ion-icon name="trash-bin-outline" class="binIcon" @click="deleteItem(item)"></ion-icon></p>
            </div>
         </div>
      </div>
      <div class="displayBtns">
         <button class="btnClearAll btn" :hidden="itemList.size < 1 ? true : false" @click="clearAllItem()">Clear All</button>
         <button class="btnSort btn" :hidden="itemList.size < 2 ? true : false" @click="sortItems()">Sort All</button>
      </div>
   </div>
</template>
<script setup>
   import { ref } from 'vue';
   const item = ref('');
   const amount = ref('');
   const unit = ref('');

   let itemList = ref(new Set());
   let sortedItemList = ref(new Set());

   const addItem = () => {
      itemList.value.add(amount.value + ' ' + unit.value + ' ' + item.value.toUpperCase());
      localStorage.setItem(`${item.value.toUpperCase()}`, `${amount.value + ' ' + unit.value}`);
      item.value = '';
   };
   const deleteItem = item => {
      try {
         const a = item.split(' ');
         localStorage.removeItem(a[2]);
         itemList.value.delete(item);
         sortedItemList.value.delete(item);
      } catch (err) {
         if (err === TypeError) return;
      }
   };
   const showItem = () => {
      const storage = { ...localStorage };
      for (const k in storage) {
         itemList.value.add(storage[k] + ' ' + k);
      }
   };
   const clearAllItem = () => {
      try {
         localStorage.clear();
         itemList.value.clear();
         sortedItemList.value.clear();
      } catch (err) {
         if (err === TypeError) return;
      }
   };
   const sortItems = () => {
      try {
         const itemArray = Array.from(itemList.value);
         const sorted = itemArray.sort().forEach(x => sortedItemList.value.add(x));
         return sortedItemList.value;
      } catch (err) {
         if (err === TypeError) return;
      }
   };
</script>
<style>
   .main {
      display: flex;
      flex-direction: column;
   }
   .input {
      width: 30%;
      display: flex;
      flex-direction: column;
      align-content: center;
   }
   .input .inputFields {
      display: flex;
      gap: 2rem;
   }
   .input input,
   .input select {
      border: none;
      border-radius: 10px;
      padding: 0.8rem;
      margin: 2rem 0;
      background-color: #a1a1a1;
      font-size: 14px;
   }
   .input .item {
      width: 75%;
   }
   .input .amount {
      width: 25%;
   }
   .btn {
      padding: 0.8rem 1.6rem;
      border-radius: 10px;
      font-size: 14px;
      border: none;
      position: relative;
      color: #323232;
   }
   .display {
      width: 20%;
      display: flex;
      flex-direction: column;
      gap: 1rem;
      font-size: 1.8rem;
      color: #aaa;
   }
   .binIcon {
      width: 1.8rem;
   }
   .elem {
      display: flex;
      gap: 1rem;
      border-bottom: 1px solid #aaa;
   }
   .displayBtns {
      display: flex;
      justify-content: space-around;
   }
   .btnClearAll,
   .btnSort {
      width: 40%;
   }
   .btnAdd {
      left: 50%;
      transform: translateX(-50%);
      width: 50%;
   }
</style>
