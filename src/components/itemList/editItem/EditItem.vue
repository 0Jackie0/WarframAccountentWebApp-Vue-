<template>
    <div class="editArea">
        <div class="titleArea">
            <span class="titleStyle">{{isAddNew === false ? "Edit Item" : "Add Item"}}</span>
        </div>

        <div class="topArea">
            <input type="file" ref='imageInput' accept=".jpg, .png" multiple={false} @change="fileChange" :style="{display: 'none'}"/>
            
            <div class="itemImageArea">
                <img :src="imageToshow" alt="item" v-on:click="changeImage"/>
            </div>

            <div class="nameQuantityArea">
                <span class="tagStyle, errorMessage">{{errorMessage}}</span>

                <span class="editFieldArea">
                    <span class="tagStyle">Item name: </span>
                    <input type="text" placeholder="Item Name" v-model="target.name" @keyup="changeName"/>
                </span>

                <span class="editFieldArea">
                    <span class="tagStyle">Item quantity: </span>
                    <input type="number" v-model="target.quantity" @keyup="changeQuantity" min="0"/>
                </span>
            </div>
        </div>

        <div class="midArea">
            <span class="editFieldArea">
                <span class="tagStyle">Item type: </span>
                <select class="dropdownStyle" :value='target.type' @change="changeType">
                    <option v-for="type in typeList" :id='type.typeId' v-bind:key='type.typeId' :value='type.typeId'>{{type.typeName}}</option>
                </select>
            </span>
            
            <span class="editFieldArea">
                <span class="tagStyle">Exceptected price: </span>
                <input type="number" v-model="target.eprice" @keyup="changeEPrice" min="0"/>
            </span>

            <span class="editFieldArea">
                <span class="tagStyle">Base price: </span>
                <input type="number" v-model="target.bprice" @keyup="changeBPrice" min="0"/>
            </span>
        </div>

        <div class="bottomArea">
            <button class="warningButton" v-if="!isAddNew" v-on:click="$emit('deletefunction', target.itemId)">Delete</button>
            <div class="saveCancleArea">
                <button class="normalButton" v-on:click="() => {this.errorMessage = ''; this.$emit('canclefunction');}">Cancle</button>
                <button class="normalButton" v-on:click="saveButtonFunctionCheck">Save</button>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

const MAX_FILE_SIZE = 2 * 1024 * 1024;


export default Vue.extend({
    name: "EditItem",
    props: ["isAddNew","typeList","target"],
    data ()
    {
        const itemImage = require('../../../drawable/220px-Warframe_Cover_Art.png');
        return {
            errorMessage: "",
            goodItem: this.target.name !== "",
            itemImage,
        }
    },

    computed:{
        imageToshow()
        {
            return this.target.imageString !== '' ? 'data:image/png;base64,' + this.target.imageString : this.itemImage;
        }
    },
    methods: {
        changeName(event)
        {
            let tempTarget = {...this.target}

            tempTarget.name = event.target.value;

            let message = "";
            let itemCheck = true;

            if (event.target.value === "")
            {
                message = "Item name cannot be empty!";
                itemCheck = false;
            }

            this.target = tempTarget;
            this.errorMessage = message;
            this.goodItem = itemCheck;
        },
        
        changeQuantity(event)
        {
            let tempTarget = {...this.target}

            if(event.target.value !== "")
            {
                tempTarget.quantity = parseInt(event.target.value, 10);
            }
            else
            {
                tempTarget.quantity = 0;
            }
            
            this.target = tempTarget;
        },
        
        changeEPrice(event) 
        {
            let tempTarget = {...this.target}

            if(event.target.value !== "")
            {
                tempTarget.eprice = parseInt(event.target.value, 10);
            }
            else
            {
                tempTarget.eprice = 0;
            }

            console.log(tempTarget.eprice);

            this.target = tempTarget
        },
        
        changeBPrice(event)
        {
            let tempTarget = {...this.target}

            if(event.target.value !== "")
            {
                tempTarget.bprice = parseInt(event.target.value, 10);
            }
            else
            {
                tempTarget.bprice = 0;
            }

            this.target = tempTarget
        },

        changeType(event)
        {
            let tempTarget = {...this.target}

            tempTarget.type = event.target.value;

            console.log(tempTarget.type);

            this.target = tempTarget;
        },

        changeImage()
        {
            this.$refs.imageInput.click();
        },

        fileChange(event)
        {
            event.stopPropagation();
            event.preventDefault();

            if (event.target.files[0])
            {
                if (event.target.files[0].size > MAX_FILE_SIZE)
                {
                    alert("The image has to be smaller than 2M");
                }
                else
                {
                    const promise = new Promise((resolve, reject) => {
                        const reader = new FileReader()
                    
                        reader.readAsDataURL(event.target.files[0])
                    
                        reader.onload = () => 
                        {
                            if (reader.result) 
                            {
                                resolve(reader.result)
                            }
                            else 
                            {
                                reject(Error("Failed converting to base64"))
                            }
                        }
                    
                    });

                    promise.then(result => {
                        const targetCopy = {...this.target};

                        targetCopy.imageString = String(result).split(",")[1];
        
                        this.target = targetCopy
                    }, 
                    err => {
                        console.log(err)
                    })
                }
            }
        },
        saveButtonFunctionCheck()
        {
            if(this.goodItem === false)
            {
                this.errorMessage = "Item name cannot be empty!";
            }
            else
            {
                this.$emit('savefunction', {target: this.target, isAddNew: this.isAddNew});
            }
        }
    },
})
</script>


<style scoped>
.errorMessage
{
    color: red;
}

.titleArea
{
    text-align: center;
    font-weight: bolder;
    margin-bottom: 10px;
}
.titleStyle
{
    font-family: 'Lobster Two', cursive;
    font-size: 30px;
}

input
{
    font-size: 20px;
}

.editFieldArea
{
    margin-top: 10px;
    margin-bottom: 10px;
}

.topArea
{
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
}
.itemImageArea
{
    background-color: #7ca1b6;
}
.itemImageArea img
{
    width: 300px;
    height: 300px;
}

.nameQuantityArea
{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-end;
}

.midArea
{
    margin-right: 20px;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-end;
}

.bottomArea
{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}


.normalButton
{
    font-size: 20px;
    font-family: 'Orbitron', sans-serif;

    padding: 5px 20px;

    background-color: white;
    color: black;
    border-radius: 12px;
    border: 2px solid #0084ff;
}
.normalButton:hover
{
    background-color: #0084ff7c;
    color: black;
    border-radius: 12px;
    border: 2px solid white;
}
.warningButton
{
    font-size: 20px;
    font-family: 'Orbitron', sans-serif;

    padding: 5px 20px;

    background-color: rgb(255, 79, 79);
    color: black;
    border-radius: 12px;
    border: 2px solid white;
}
.warningButton:hover
{
    background-color: white;
    color: black;
    border-radius: 12px;
    border: 2px solid rgb(255, 79, 79);
}

.saveCancleArea
{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}
</style>