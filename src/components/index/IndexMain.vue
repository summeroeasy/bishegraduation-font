<template>
	<el-main>
		<bread-crumbs :title="title" class="bread-crumbs"></bread-crumbs>
		<router-view class="router-view"></router-view>
	</el-main>
</template>
<script>
	import menu from "@/utils/menu";
	export default {
		data() {
			return {
				menuList: [],
				role: "",
				currentIndex: -2,
				itemMenu: [],
				title: ''
			};
		},
		mounted() {
			let menus = menu.list();
			this.menuList = menus;
			this.role = this.$storage.get("role");
		},
		methods: {
			menuHandler(menu) {
				this.$router.push({
					name: menu.tableName
				});
				this.title = menu.menu;
			},
			titleChange(index, menus) {
				this.currentIndex = index
				this.itemMenu = menus;
				console.log(menus);
			},
			homeChange(index) {
				this.itemMenu = [];
				this.title = ""
				this.currentIndex = index
				this.$router.push({
					name: 'home'
				});
			},
			centerChange(index) {
				this.itemMenu = [{
					"buttons": ["新增", "查看", "修改", "删除"],
					"menu": "修改密码",
					"tableName": "updatePassword"
				}, {
					"buttons": ["新增", "查看", "修改", "删除"],
					"menu": "个人信息",
					"tableName": "center"
				}];
				this.title = ""
				this.currentIndex = index
				this.$router.push({
					name: 'home'
				});
			}
		}
	};
</script>
<style lang="scss" scoped>
a {
    text-decoration: none;
    color: #555;
    &:hover {
        background: #5CACEE; // 柔和的蓝色
        color: #fff;
    }
}

.nav-list {
    width: 100%;
    margin-top: 20px;
    .nav-title {
        display: block;
        font-size: 16px;
        color: #333;
        padding: 15px 25px;
        transition: background-color .3s, color .3s;
    }

    .nav-title.active {
        color: #5CACEE;
        cursor: default;
        background-color: #f0f4f7; // 淡蓝色的背景
        border-left: 4px solid #5CACEE;
    }
}

.nav-item {
    background: #FFFFFF;
    padding: 15px 0;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);

    .menu {
        padding: 15px 25px;
        &:hover {
            background: #ebf5ff; // 非常淡的蓝色
        }
    }
}

.el-main {
    background-color: #F6F8FA;
    padding: 24px;
    min-height: calc(100vh - 60px);
}

.router-view {
    padding: 20px;
    margin-top: 20px;
    background: #FFFFFF;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.bread-crumbs {
    padding: 10px 25px;
    background-color: #fff;
    border-bottom: 1px solid #e9eef3;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    margin-bottom: 20px;
}
</style>
