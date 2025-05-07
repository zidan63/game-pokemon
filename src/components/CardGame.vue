<template>
    <div class="card" :class="{'disable': isDisable , 'disabletemp':isDisableTemp}"
    :style="{
       height: `${(920 - 16 * 4)/Math.sqrt(cardReady.length)-16}px`,
       width: `${((920 - 16 * 4)/Math.sqrt(cardReady.length)-16) * 3 / 4}px` ,
       perpective: `${((920 - 16 * 4)/Math.sqrt(cardReady.length)-16) * 3 / 2 }px`
    }">
        <div class="card_inner" 
        :class="{'isflipped':isFlipped }" 
        @click="onToggleFlippedCard()">
        <!-- 
            * :class : toggel class isflipped và thẻ div khi biến isFlipped là true
            * @click="ontoggle" : để chuyển isFlipped thành true hoặc false với mục đích lật thẻ

        -->
            <div class="card_face card_front">
                <div class="card_content" :style="{backgroundSize: `${((920 - 16 * 4)/Math.sqrt(cardReady.length)-16) * 3 / 4 /3}px ${((920 - 16 * 4)/Math.sqrt(cardReady.length)-16) * 3 / 4 /3}px`}">

                </div>
            </div>
            <div class="card_face card_back">
                <div class="card_content" :style="{backgroundImage:`url(${require('@/assets' + imgBackFace)})`}">
                    
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    props:{
        card: {
            type: [String, Number, Array, Object],
        },
        imgBackFace:{
            type: String,
            require: true,
        },
        cardReady:{
            type: [],
            default: function(){
                return [];
            }

        }
    },
    data(){
        return {
            isFlipped: false,
            isDisable: false,
            isDisableTemp: false,
        }
    },
    methods:{   
        onToggleFlippedCard( ){
            if(this.isDisable) return false;
            if(this.isDisableTemp) return false;
            this.isFlipped = !this.isFlipped;
            if(this.isFlipped) this.$emit("onFlip",this.card)
        },
        closeFlipCard(){
            this.isFlipped = false;
        },
        onDisable (){
            this.isDisable= true;
        },

        onDisableTemp(){
            this.isDisableTemp = true;

        },
        offDisableTemp(){
            this.isDisableTemp = false;
        }
        
    },  
}
</script>

<style lang="css" scoped>
    .card{
        display: inline-block;
        margin-right: 1rem;
        margin-bottom: 1rem;
        

    }
    .card_inner{
        width: 100%;
        height: 100%;
        transition: transform 1s;
        transform-style: preserve-3d;
        cursor: pointer;
        position: relative;
    }


    .card_inner.isflipped{
        transform: rotateY(-180deg);
    }

    .card_face {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        overflow: hidden;
        border-radius: 1rem;
        padding: 1rem;
        box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
    }

    .card_back{
        background-color: var(--light);
        transform: rotateY(-180deg);

    }

    .card_front .card_content{
        background: url("../assets/img/icon_back.png") no-repeat center center;
        background-size: 40px 40px;
        height: 100%;
        width: 100%;
    }

    .card_back .card_content{
        background-size: contain;
        background-position: center;
        background-repeat: no-repeat;
        height: 100%;
        width: 100%;
    }
    
    .card.disable .card_inner{
        cursor: default;
    }

    .card.disabletemp .card_inner{
        cursor: default;
    }
    
</style>
