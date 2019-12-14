<style>
    #editForm {width:800px;margin: 0 auto; margin-top:80px;}
    .my-2 {width:500px; text-align: center;}
    ul, ol {list-style:none; padding-left: 0px; }
    .ul-style li {list-style-type: none; float: left;}
    .mx-auto {margin-top:50px}
    .list-sub>span:first-child {width:150px;padding-right:50px;}
    .list-sub>span:last-child {width:150px}
    .list-btn>span:first-child {margin-right:10px}
</style>
<template>
    <div id="editForm">
        <div>
            <v-row>
                <v-col cols="6" sm="4">
                    <v-text-field label="이름" ref="name" v-model="name" class="edit-input" single-line solo></v-text-field>
                </v-col>
                <v-col cols="6" sm="4">
                    <v-text-field label="비밀번호" ref="pw" v-model="pw" class="edit-input" single-line solo></v-text-field>
                </v-col>
            </v-row>    
            <v-row>
                <v-col cols="12" md="8">
                    <v-textarea
                                solo
                                v-model="text"
                                name="input-7-4"
                                class="edit-text"
                                ref="text"
                                label="내용">
                    </v-textarea>
                </v-col>
            </v-row>

            <div class="my-2" >
                <v-btn middle @click="editBtn" id="editBtn" idx="" >{{editMode}}</v-btn>
            </div>
        </div>

        <!-- 리스트 목록 -->
        <v-card 
                max-width="530"
                class="mx-auto" >
            <v-list two-line v-if="listCnt">
                <v-list-item v-for="(item, idx) in items.slice().reverse()" :key="idx">
                <v-list-item-content>
                    <v-list-item-title>{{item.text}}</v-list-item-title>
                    <v-list-item-subtitle class="list-sub">
                        <span>{{item.name}}</span>
                        <span>{{item.date}}</span>
                    </v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-icon class="list-btn">
                    <span><v-btn small class="vbtn" @click="update(items.length - idx -1)">수정</v-btn></span>
                    <span><v-btn small class="vbtn" @click="remove(items.length - idx-1)">삭제</v-btn></span>
                </v-list-item-icon>
                </v-list-item>
            </v-list>
        </v-card>
    </div>
</template>

<script>
export default {
    data() {
        var today = new Date();
        var dd = today.getDate();
        var mm = today.getMonth()+1; //January is 0!
        var yyyy = today.getFullYear();

        return {
            today: yyyy+"-"+mm+"-"+dd,
            editMode: "등록하기",
            listCnt:0,
            items : [],
            name:"",
            pw:"",
            text:""
        }
    }
    , methods: {
        editBtn() {
            var obj = {};
            obj.name = this.name;
            obj.date = this.today;
            obj.pw = this.pw;
            obj.text = this.text;

            if(obj.name == "") {
                alert("이름을 입력하세요");
                this.$refs.name.focus();
                return false;
            }
            if(obj.pw == "") {
                alert("비밀번호를 입력하세요");
                this.$refs.pw.focus();
                return false;
            }
            if(obj.text == "") {
                alert("내용을 입력하세요");
                this.$refs.text.focus();
                return false;
            }
            if(this.editMode == "등록하기") {
                this.items.push(obj);
            } else {
                //수정하기
                var idx = editBtn.getAttribute('idx');
                var obj = this.items[idx];
                if(obj.pw != this.pw) {
                    alert("비밀번호가 일치하지 않습니다.");
                    this.$refs.pw.focus();
                    return false;
                }
                obj.name = this.name;
                obj.pw = this.pw;
                obj.date = this.date;
                obj.text = this.text;
                editBtn.setAttribute('idx',"");
                alert("수정되었습니다.");
            }
            
            this.updateSet('add');
            
        },
        update(idx) {
            var obj = this.items[idx];
            this.name = obj.name;
            this.date = obj.date;
            this.text = obj.text;
            editBtn.setAttribute('idx', idx);
            this.updateSet('update');
        },
        remove(idx) {
            this.items.splice(idx,1);
            this.updateSet('remove');
        },
        updateSet(mode) {
            this.listCnt = this.items.length;
            if(mode == "add") { 
                this.editMode = "등록하기";
                this.name = "";
                this.pw = "";
                this.text = "";
            } else if(mode == "update") {
                this.editMode = "수정하기";
            } else {
                this.name = "";
                this.pw = "";
                this.text = "";
            }
        }
    }

}
</script>