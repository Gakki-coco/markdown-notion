<template>
	<span :title="username">{{slug}}</span>
</template>

<script>
import Auth from '@/apis/auth'
import Bus from '@/helpers/eventBus'
export default {
	data() {
		return {
			username: '未登录'
		}
    },
    created() {
        Bus.$on('userInfo', user => {
            this.username = user.username
        })
        
        Auth.getInfo()
            .then(response => {
                console.log(response)
                if (response.isLogin) {
                    this.username = response.data.username
                }else {
                    let { path } = this.$router.history.current
					if (path !== '/login') {
						this.$router.push({ path: '/login' })
					}
                }
            })
    },
    computed: {
        slug() {
            return this.username.charAt(0)
        }
    }
}
</script>

<style lang="scss" scoped>
span {
	display: inline-block;
	width: 30px;
	height: 30px;
	text-align: center;
	line-height: 32px;
	border-radius: 50%;
	background: #f2b81c;
	color: #fff;
	text-shadow: 1px 0 1px #795c19;
	font-weight: bold;
	text-transform: uppercase;
	font-size: 18px;
	margin-top: 15px;
}
</style>