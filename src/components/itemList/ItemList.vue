<template>
    <div class="itemListPageArea">
        <!-- {this.createSellString === true ? 
            <div class="sellStringCreationArea">
                <SellString typeList={this.itemType} itemList={this.itemListCopy === null ? this.itemList : this.itemListCopy} closeFunction={this.openCLoseSellString}/>
            </div> 
            :
            null
        } -->
        
        <div class="selectionArea">
            <ListSelection v-bind:filterList='itemType' v-bind:orderList='orderList' v-on:searchfunction="searchItem" v-on:filterfunction="filterItemList" v-on:orderfunction="orderItemList" v-on:opensellstring="openCLoseSellString"/>
        </div>


        <div class="itemManagementArea">
            <div class="itemManagementContent">
                <div class="editAreaWapper">
                    <EditItem v-if="editTarget != null" v-bind:isAddNew="addNew" v-bind:target="editTarget" v-bind:typeList="itemType" v-on:savefunction="saveEdit" v-on:deletefunction="deleteEdit" v-on:canclefunction="cancleEdit" />
                </div>
            </div>

            <div class="itemManagementContent">
                <div class="listInfoArea">
                    <div class="listHeaderArea">
                        <ListHeader v-bind:itemList="itemList" />
                    </div>

                    <div class="listContentArea">
                        <ListContent v-for="item in itemList" :key="item.itemId" v-bind:item="item" v-bind:typeName="getItemTypeNameById(item.type)" v-on:addoneitem="addOneItem"  v-on:removeoneitem="removeOneItem" v-on:openedit="openEdit" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import ServerCommunication from '../../communication/ServerCommunication.js';
    import ListContent from './listContent/ListContent.vue';
    import EditItem from './editItem/EditItem.vue';
    import ListHeader from './listHeader/ListHeader.vue';
    import ListSelection from './listSelection/ListSelection.vue';

    export default Vue.extend({
        name: "ItemList",
        components:{
            ListContent,
            EditItem,
            ListHeader,
            ListSelection
        },
        data ()
        {
            const orderList = [
                {
                    orderId: 1,
                    orderName: "Name"
                },
                {
                    orderId: 2,
                    orderName:"Quantity"
                }
            ];

            return {
                itemList: [],
                itemType: [],
                itemListCopy: null,
                editTarget: null,
                addNew: true,
                filterId: "-1",
                orderId: "1",
                createSellString: false,
                orderList: orderList
            }
        },
        computed:{

        },
        created() 
        {
            this.getItemList();
            this.getItemTypeList()

        },
        methods: {
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            getItemList ()
            {
                const serverCommunication = new ServerCommunication();

                serverCommunication.itemCommunication().getAllItemName().then(newItemList =>
                    {
                        this.itemList = newItemList.data
                    }
                )
            },
            getItemTypeList ()
            {
                const serverCommunication = new ServerCommunication();

                serverCommunication.typeCommunication().getAllType().then(newTypeList =>
                    {
                        this.itemType = newTypeList.data;
                        this.editTarget = {
                            imageString: "",
                            bprice: 0,
                            eprice: 0,
                            itemId: -1,
                            name: "",
                            quantity: 0,
                            type: newTypeList.data[0].typeId,
                        }
                    }
                )
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            openCLoseSellString()
            {
                this.createSellString = !this.createSellString;
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            searchItem(event)
            {
                let searchName = event.target.value;
                if(searchName === "")
                {
                    this.itemList = this.itemListCopy;
                    this.itemListCopy = null;
                }
                else
                {
                    let newItemList = [];
                    let newItemListCopy;

                    if(this.itemListCopy !== null)
                    {
                        newItemListCopy = this.itemListCopy;
                    }
                    else
                    {
                        newItemListCopy = this.itemList;
                    }
                    

                    for(let index in newItemListCopy)
                    {
                        if(newItemListCopy[index].name.toLowerCase().includes(searchName.toLowerCase()) === true)
                        {
                            newItemList.push(newItemListCopy[index])
                        }
                    }

                    if(this.itemListCopy !== null)
                    {
                        this.itemList = newItemList;
                    }
                    else
                    {
                        this.itemList = newItemList;
                        this.itemListCopy = newItemListCopy;
                    }
                }
            },
            filterItemList(event)
            {
                let typeId = event.target.value;
                const serverCommunication = new ServerCommunication();

                if(this.orderId === "1")
                {
                    if(typeId === "-1")
                    {
                        serverCommunication.itemCommunication().getAllItemName().then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.filterId = typeId;
                            }
                        )
                    }
                    else
                    {
                        serverCommunication.itemCommunication().getFilterNameItemList(typeId).then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.filterId = typeId;
                            }
                        )
                    }
                }
                else
                {
                    if(typeId === "-1")
                    {
                        serverCommunication.itemCommunication().getAllItemQuantity().then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.filterId = typeId;
                            }
                        )
                    }
                    else
                    {
                        serverCommunication.itemCommunication().getFilterQuantityItemList(typeId).then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.filterId = typeId;
                            }
                        )
                    }
                }
            },
            orderItemList(event)
            {
                let orderId = event.target.value;
                const serverCommunication = new ServerCommunication();

                if(orderId === "1")
                {
                    if(this.filterId === "-1")
                    {
                        serverCommunication.itemCommunication().getAllItemName().then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.orderId = orderId;
                            }
                        )
                    }
                    else
                    {
                        serverCommunication.itemCommunication().getFilterNameItemList(this.filterId).then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.orderId = orderId;
                            }
                        )
                    }
                    
                }
                else
                {
                    if(this.filterId === "-1")
                    {
                        serverCommunication.itemCommunication().getAllItemQuantity().then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.orderId = orderId;
                            }
                        )
                    }
                    else
                    {
                        serverCommunication.itemCommunication().getFilterQuantityItemList(this.filterId).then(newItemList =>
                            {
                                this.itemList = newItemList.data;
                                this.orderId = orderId;
                            }
                        )
                    }
                }
            },
            getNewFilterList() 
            {
                const serverCommunication = new ServerCommunication();

                if(this.orderId === "1")
                {
                    if(this.filterId === "-1")
                    {
                        serverCommunication.itemCommunication().getAllItemName().then(newItemList =>
                            {
                                this.itemList = newItemList.data
                            }
                        )
                    }
                    else
                    {
                        serverCommunication.itemCommunication().getFilterNameItemList(this.filterId).then(newItemList =>
                            {
                                this.itemList = newItemList.data
                            }
                        )
                    }
                    
                }
                else
                {
                    if(this.filterId === "-1")
                    {
                        serverCommunication.itemCommunication().getAllItemQuantity().then(newItemList =>
                            {
                                this.itemList = newItemList.data
                            }
                        )
                    }
                    else
                    {
                        serverCommunication.itemCommunication().getFilterQuantityItemList(this.filterId).then(newItemList =>
                        {
                            this.itemList = newItemList.data
                            }
                        )
                    }
                }
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////



            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            getItemTypeNameById(target) 
            {
                for(let index = 0; index < this.itemType.length; index ++)
                {
                    if(target === this.itemType[index].typeId)
                    {
                        return this.itemType[index].typeName;
                    }
                }
                return "";
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////



            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            addOneItem(itemId)
            {
                const serverCommunication = new ServerCommunication();

                serverCommunication.itemCommunication().changeOneItemQuantity(itemId, 1).then(() =>
                    {
                        const tempItemList = {...this.itemList}

                        for(let itemIndex in tempItemList)
                        {
                            if(tempItemList[itemIndex].itemId === itemId)
                            {
                                tempItemList[itemIndex].quantity += 1;
                                break;
                            }
                        }

                        this.itemList = tempItemList;
                    })
            },
            removeOneItem(itemId)
            {
                const serverCommunication = new ServerCommunication();

                serverCommunication.itemCommunication().changeOneItemQuantity(itemId, -1).then(() =>
                {
                    const tempItemList = {...this.itemList}
                    
                    for(let itemIndex in tempItemList)
                    {
                        if(tempItemList[itemIndex].itemId === itemId)
                        {
                            tempItemList[itemIndex].quantity += -1;
                            break;
                        }
                    }
                    this.itemList = tempItemList;
                })
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////



            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            openEdit(targetItem)
            {
                this.editTarget = targetItem;
                this.addNew = false;
            },
            cancleEdit()
            {
                this.resteEditTarget();
            },
            saveEdit(respond)
            {
                let target = respond.target;
                let isAddNew = respond.isAddNew;

                const serverCommunication = new ServerCommunication();

                if(isAddNew === false)
                {
                    serverCommunication.itemCommunication().saveEdit(target).then(() =>
                    {
                        this.resteEditTarget();

                        this.getNewFilterList();
                    })
                    .catch(exception => {
                        console.log(exception)
                    })
                }
                else
                {
                    serverCommunication.itemCommunication().addNewItem(target).then(() =>
                    {
                        this.resteEditTarget();
        
                        this.getNewFilterList();
                    })
                    .catch(exception => {
                        console.log(exception)
                    })
                }
            },
            deleteEdit(targetId)
            {
                const serverCommunication = new ServerCommunication();

                console.log("remove " + targetId)
                serverCommunication.itemCommunication().removeItem(targetId).then(() =>
                {
                    this.resteEditTarget();
                    this.getNewFilterList();
                })
                .catch(exception =>
                {
                    console.log(exception)
                })
            },
            resteEditTarget()
            {
                
                this.editTarget = {
                    imageString: "",
                    bprice: 0,
                    eprice: 0,
                    itemId: -1,
                    name: "",
                    quantity: 0,
                    type: this.itemType[0].typeId,
                };
                this.addNew = true;
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
        }
    })
</script>

<style>
.itemListPageArea
{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
}

.sellStringCreationArea
{
    z-index: 2;
    position: absolute;
    top: 15%;
    left: 20%;
    right: 20%;
}

.itemManagementArea
{
    height:  80vh;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 25px;
}

.itemManagementContent
{
    display:inline-block;
    width: 49%;
}

.editAreaWapper
{
    /* height: 615px; */
    height: 100%;

    background-color: #d3eaf7;
    
    border-style: dotted;
    border-color: #67abd6dc;
    border-width: 3px;
}

.listInfoArea
{
    background-color: #d3eaf7;
    
    border-style: dotted;
    border-color: #67abd6dc;
    border-width: 3px;
}

.listContentArea
{
    overflow-y: auto;
    height:  75.5vh;
    /* height: calc(100%); */
}
</style>