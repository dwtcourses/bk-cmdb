<template>
    <div class="tips-wrapper">
        <div class="content-wrapper">
            <div class="title">
                <img src="../../assets/images/full-text-search.png" alt="no-data">
                <h2>{{$t('业务不存在或无权限')}}</h2>
            </div>
            <div class="btns">
                <bk-button theme="primary" @click="handleApplyPermission" :loading="$loading('getSkipUrl')">
                    {{$t('申请功能权限')}}
                </bk-button>
            </div>
        </div>
    </div>
</template>
<script>
    import { translateAuth } from '@/setup/permission'
    export default {
        data () {
            return {
                permission: []
            }
        },
        watch: {
            $route: {
                immediate: true,
                handler () {
                    this.setPermission()
                }
            }
        },
        methods: {
            async setPermission () {
                const permission = []
                const operation = this.$tools.getValue(this.$route.meta, 'auth.operation', {})
                if (Object.keys(operation).length) {
                    const translated = await translateAuth(Object.values(operation))
                    permission.push(...translated)
                }
                this.permission = permission
            },
            async handleApplyPermission () {
                try {
                    const skipUrl = await this.$store.dispatch('auth/getSkipUrl', {
                        params: this.permission,
                        config: {
                            requestId: 'getSkipUrl'
                        }
                    })
                    window.open(skipUrl)
                } catch (e) {
                    console.error(e)
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
    .tips-wrapper {
        overflow: hidden;
    }
    .content-wrapper {
        margin-top: 100px;
        text-align: center;
        color: #63656E;
        font-size: 14px;
        .title {
            img {
                width: 136px;
            }
            h2 {
                margin-bottom: 10px;
                font-size: 22px;
                color: #313238;
                font-weight: normal;
            }
            p {
                color: #63656e;
                font-size: 14px;
            }
        }
        .btns {
            margin-top: 24px;
            .bk-button {
                border-radius: 3px;
                padding-left: 10px;
                padding-right: 10px;
            }
        }
    }
</style>
