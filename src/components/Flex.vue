<template>
    <div class="Flex">
        <drop-list :items="items" class="dl" :style="{flexDirection: direction}" @insert="onInsert"
                   @reorder="onReorder" mode="cut">
            <template v-slot:item="{item}">
                <drag tag="generic" :model="item" :key="key(item)" :data="item" @mousedown.native.stop=""
                      @cut="remove(item)">
                    <template v-slot:drag-image>
                        <div class="drag-image">DRAG</div>
                    </template>
                </drag>
            </template>
            <template v-slot:feedback="{data}">
                <span :key="key(data)">feedback</span>
            </template>
        </drop-list>
    </div>
</template>

<script lang="ts">
    import DropList from "../../lib/src/components/DropList.vue";
    import Drag from "../../lib/src/components/Drag.vue";
    import {Component, Prop, Vue} from "vue-property-decorator";
    import {InsertEvent, ReorderEvent} from "../../lib/src/ts/events";

    @Component({
        name: 'Flex',
        components: {Drag, DropList}
    })
    export default class Flex extends Vue {

        @Prop()
        items: any[];

        @Prop()
        direction: string;

        key(item) {
            if (typeof item === "object") {
                return item.key
            } else {
                return item;
            }
        }

        onInsert(event: InsertEvent) {
            this.items.splice(event.index, 0, event.data);
        }

        onReorder(event: ReorderEvent) {
            event.apply(this.items);
        }

        remove(item) {
            let index = this.items.indexOf(item);
            this.items.splice(index, 1);
        }

    }
</script>

<style scoped lang="scss">
    .Flex {
        border: 1px dashed black;

        .dl {
            display: flex;
            align-items: stretch;
        }

        .drag-image {
            background-color: orangered;
            width: 75px;
            height: 75px;
            transform: translate(-50%, -50%);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    }
</style>