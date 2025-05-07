<template>
    <div class="screen">
        <div class="back" @click="backToDefaul()">
            <button>Back</button>
        </div>
        <div class="screen_inner" :style="{
            width: `${(((920 - 16 * 4)/Math.sqrt(cardReady.length)-16) * 3 / 4 +16)*Math.sqrt(cardReady.length)}px`
        }">
        
            <card-flip 
            v-for="(card, index) in cardReady " 
            :key="index"  
            :imgBackFace="`/img/${card}.png`"
            :ref="`card-${index}`"
            :card="{index , value: card}"
            @onFlip="checkRules($event)"
            :cardReady="cardReady"
        />
        </div>
    </div>
</template>

<script>
import CardFlip from "./CardGame.vue"
export default {
    props: {
        cardReady: {
            type: Array,
            default: function() {
                return [];
            }
        },
    },

    components:{
        CardFlip,
    },
    data(){
        return {
            rules: [],
        };
    },
    methods:{
        checkRules(card){
            
            //nếu đã chọn 2 thì disable tất cả các thẻ
            if(this.rules.length === 1 ){
                for (let i = 0; i < this.cardReady.length; i++) {
                    if (this.$refs[`card-${i}`][0]) {
                        this.$refs[`card-${i}`][0].onDisableTemp();
                    }
                }
                setTimeout(() => {
                    for (let i = 0; i < this.cardReady.length; i++) {
                    if (this.$refs[`card-${i}`][0]) {
                        this.$refs[`card-${i}`][0].offDisableTemp();
                    }
                }
                }, 900);
            }
            
            this.rules.push(card);

            //nếu chọn 1 thì sẽ disable thẻ vừa chọn
            if (this.rules.length === 1) {
                this.$refs[`card-${this.rules[0].index}`][0].onDisableTemp();
            }
            //nếu chọn 2 thì disable 2 thẻ vừa chọn sau khi 0,9s(900ms) thì bỏ disable cả 2 thẻ
            if (this.rules.length === 2) {
                const index1 = this.rules[0].index;
                const index2 = this.rules[1].index;
                if (this.$refs[`card-${index1}`][0]) {
                    this.$refs[`card-${index1}`][0].onDisableTemp();
                }
                if (this.$refs[`card-${index2}`][0]) {
                    this.$refs[`card-${index2}`][0].onDisableTemp();
                }

                setTimeout(() => {
                    if (this.$refs[`card-${index1}`][0]) {
                        this.$refs[`card-${index1}`][0].offDisableTemp();
                    }
                    if (this.$refs[`card-${index2}`][0]) {
                        this.$refs[`card-${index2}`][0].offDisableTemp();
                    }
                }, 900);
                
            }
            //trường hợp đúng
            if(this.rules.length === 2 && this.rules[0].value===this.rules[1].value){
                //right
                // add class tới component card
                this.$refs[`card-${this.rules[0].index}`][0].onDisable();
                this.$refs[`card-${this.rules[1].index}`][0].onDisable();
                
                //reset rules
                this.rules = [];

                //check đã xong game chưa
                const disableElements = document.querySelectorAll(".screen .card.disable");

                if(disableElements && disableElements.length == this.cardReady.length -2){
                    // gửi sự kiện cho app biết là game đã xong
                    setTimeout(() => {
                        this.$emit("onFinish")
                    }, 1000);
                    
                }
            }// trường hợp sai
            else if(this.rules.length === 2 && this.rules[0].value !==this.rules[1].value){
                setTimeout(() => {
                    // đóng 2 thẻ
                    this.$refs[`card-${this.rules[0].index}`][0].closeFlipCard();
                    this.$refs[`card-${this.rules[1].index}`][0].closeFlipCard();
                    
                    //reset [] rules
                    this.rules = [];

                }, 900);
            }
            else return false;

        }, 
        backToDefaul(){
            this.$emit("onBack")
        }
    },
    
}
</script>



<style lang="css" scoped>
    .back{
        position: absolute;
        top: 1rem;
        left: 1rem;
    }
    .screen{
        width: 100%;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 2;
        background-color: var(--dark);
        color: var(--light);

    }
    .screen_inner{
        display: flex;
        flex-wrap: wrap;
        margin: 2rem auto;
    }
</style>