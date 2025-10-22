<script>
    export default { 
        // Add code in appropriate places
        data() {
            return {
                items: ['keyboard', 'mouse', 'iPhone', 'macbook', 'adapter']
            }
            
        },
        methods: {
        addItem() {
            if (this.newItem.trim()) {
                this.items.push(this.newItem.trim());
                this.newItem = '';
            }
        },
        deleteItem(index) {
            this.items.splice(index, 1);
        }
    }
    }
</script>

<template>

    <h2>Shopping Cart</h2>
    <ul id="items"> 
        <li v-for="(item, index) in items" :key="item">
            {{ item }}
            <button @click="deleteItem(index)">delete</button>
        </li>
    </ul>
    <input type="text" v-model="newItem" />
    <button @click="addItem">Add!</button>
   
</template>
