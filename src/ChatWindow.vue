<template>
    <div class="sc-chat-window" :class="{opened: isOpen, closed: !isOpen}">
        <!--    <Header-->
        <!--      :title="title"-->
        <!--      :imageUrl="titleImageUrl"-->
        <!--      :onClose="onClose"-->
        <!--      :colors="colors"-->
        <!--      :disableUserListToggle="disableUserListToggle"-->
        <!--      @userList="handleUserListToggle"-->
        <!--    >-->
        <!--      <template>-->
        <!--        <slot name="header">-->
        <!--        </slot>-->
        <!--      </template>-->
        <!--    </Header>-->

        <div style=" border-top-right-radius: 3px; border-top-left-radius: 3px; display: flex; align-items: flex-end; background-color: #C84C93; height: 33px "
             v-if="$store.getters.authState">
            <div :class="{'active':$store.state.currentKey===item._id}" @click="getMsgs(item)"
                 style="text-align: center;" v-for="item in $store.getters.rooms">
                <span @click="getMsgs(item)" class="rooms"
                      style="font-size: 13px; cursor: pointer">№  {{item.uniqID}}
                    <span v-if="item.unread > 0" class="unread">{{item.unread}}</span>
                </span>
            </div>
            <div class="msg_wrap">
                <img :class="{'active_msg':!$store.state.currentKey}" class="new_msg" @click="openMsgCreate"
                     src="../../../public/images/add.svg" alt="">
            </div>

        </div>

        <UserList v-if="showUserList" :participants="participants"/>
        <MessageList
                v-if="!showUserList"
                :messages="messages"
                :participants="participants"
                :showTypingIndicator="showTypingIndicator"
                :colors="colors"
                :alwaysScrollToBottom="alwaysScrollToBottom"
                :messageStyling="messageStyling"
                @scrollToTop="$emit('scrollToTop')"
                @remove="$emit('remove', $event)">

            <template v-slot:user-avatar="scopedProps">
                <slot name="user-avatar" :user="scopedProps.user" :message="scopedProps.message">
                </slot>
            </template>
            <template v-slot:text-message-body="scopedProps">
                <slot name="text-message-body" :message="scopedProps.message" :messageText="scopedProps.messageText"
                      :messageColors="scopedProps.messageColors" :me="scopedProps.me">
                </slot>
            </template>
            <template v-slot:system-message-body="scopedProps">
                <slot name="system-message-body" :message="scopedProps.message">
                </slot>
            </template>
            <template v-slot:text-message-toolbox="scopedProps">
                <slot name="text-message-toolbox" :message="scopedProps.message" :me="scopedProps.me">
                </slot>
            </template>
        </MessageList>
        <UserInput
                v-if="!showUserList"
                :showEmoji="showEmoji"
                :onSubmit="onUserInputSubmit"
                :suggestions="getSuggestions()"
                :showFile="showFile"
                :placeholder="placeholder"
                @onType="$emit('onType')"
                @edit="$emit('edit', $event)"
                :colors="colors"/>
        <NewMessage :opened="$store.state.openMessageCreate" v-on:close-it="openMessageCreate = false"></NewMessage>
    </div>
</template>

<script>
    import Header from './Header.vue'
    import MessageList from './MessageList.vue'
    import UserInput from './UserInput.vue'
    import UserList from './UserList.vue'
    import NewMessage from '@/components/NewMessageFB.vue';

    export default {
        components: {
            Header,
            MessageList,
            UserInput,
            UserList,
            NewMessage
        },
        props: {
            showEmoji: {
                type: Boolean,
                default: false
            },
            showFile: {
                type: Boolean,
                default: false
            },
            participants: {
                type: Array,
                required: true
            },
            title: {
                type: String,
                required: true
            },
            titleImageUrl: {
                type: String,
                default: ''
            },
            onUserInputSubmit: {
                type: Function,
                required: true
            },
            onClose: {
                type: Function,
                required: true
            },
            messageList: {
                type: Array,
                default: () => []
            },
            isOpen: {
                type: Boolean,
                default: () => false
            },
            placeholder: {
                type: String,
                default: 'Write a message...'
            },
            showTypingIndicator: {
                type: String,
                required: true
            },
            colors: {
                type: Object,
                required: true
            },
            alwaysScrollToBottom: {
                type: Boolean,
                required: true
            },
            messageStyling: {
                type: Boolean,
                required: true
            },
            disableUserListToggle: {
                type: Boolean,
                default: false
            }
        },
        data() {
            return {
                showUserList: false
            }
        },
        computed: {
            messages() {
                let messages = this.messageList
                return messages
            }
        },
        methods: {
            openMsgCreate() {
                this.$store.dispatch('loadStartRoom')
            },
            getMsgs(item) {
                let key = item._id
                this.$store.commit('setCurrentKey', key)
                this.$store.dispatch('loadChat', key)
            },
            handleUserListToggle(showUserList) {
                this.showUserList = showUserList
            },
            getSuggestions() {
                return this.messages.length > 0 ? this.messages[this.messages.length - 1].suggestions : []
            }
        }
    }
</script>
<style scoped>
    .new_msg {
        height: 25px;
        margin-left: 7px;
        cursor: pointer;
        margin-top: 10px;
    }

    .active_msg {
        /*background-color: white;*/
        border-bottom: 2px solid white;
        padding: 2px;
        border-radius: 2px;
    }

    .active {
        background-color: white;
        /*text-align: center;*/
        border-top-left-radius: 3px;
        border-top-right-radius: 3px;
    }

    .unread {
        position: absolute;
        background: #ff4444;
        top: 0px;
        border-radius: 10px;
        height: 18px;
        padding: 1px;
        width: 16px;
        margin-bottom: 2px;
        text-align: center;
    }

    .msg_wrap {
        justify-content: center;
        margin: 3px;
        padding: 3px;
        border-top-right-radius: 3px;
        border-top-left-radius: 3px;
        /*height: 33px;*/
        width: 30px;
        /*background-color: #C84C93;*/
        /*color: white;*/
    }

    .rooms {
        justify-content: center;
        margin: 3px;
        padding: 3px;
        border-top-right-radius: 3px;
        border-top-left-radius: 3px;
        height: 33px;
        width: 55px;
        background-color: #C84C93;
        color: white;
    }

    .sc-chat-window {
        border: 2px solid #C84C93;
        width: 350px;
        height: calc(100% - 220px);
        max-height: 590px;
        position: fixed;
        right: 25px;
        bottom: 100px;
        box-sizing: border-box;
        /*box-shadow: 0px 7px 40px 2px rgba(148, 149, 150, 0.1);*/
        background: white;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        transition: 0.3s;
        transition: 0.5s ease-in-out;
        border-radius: 7px;
        font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
    }

    .sc-chat-window.closed {
        opacity: 0;
        display: none;
        bottom: 90px;
    }

    .sc-message--me {
        text-align: right;
    }

    .sc-message--them {
        text-align: left;
    }

    @media (max-width: 450px) {
        .sc-chat-window {
            width: 100%;
            height: 100%;
            max-height: 100%;
            right: 0px;
            bottom: 0px;
            border-radius: 0px;
        }

        .sc-chat-window {
            transition: 0.1s ease-in-out;
        }

        .sc-chat-window.closed {
            bottom: 0px;
        }
    }
</style>
