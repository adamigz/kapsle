<template>
    <div>
        <b-alert show :variant="variant">{{category}}</b-alert>
    </div>
</template>
<script>
export default {
    props: {
        category: String
    },
    computed: {
        variant() {
            if(this.category == 'zwykły'){
                return 'primary';
            }else if(this.category == 'unikalny'){
                return 'success';
            } else {
                return 'warning';
            }
        }
    },

}
</script>