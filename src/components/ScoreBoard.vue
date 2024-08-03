<template>
    <section class="score">
        Player <span>{{ this.winCount }}</span> X <span>{{ this.loseCount }}</span> Computer
    </section>
</template>

<script>
    export default{
        props: ['winCount','loseCount']

    };

</script>

<style lang="scss">
    section.score{
        border-bottom: 1px solid black;
        padding: 24px;
        font-size: 18px;

        span{
            border: 1px solid black;
            padding: 5px 10px;
            font-weight: bold;
        }
    }
</style>