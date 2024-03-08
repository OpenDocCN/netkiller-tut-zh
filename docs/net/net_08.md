# 第 28 章 HP

## iLO

hp 的 ilo 卡可以通过 hp 官方提供的工具 hponcfg (HP Lights-Out Online Configuration utility)来修改密码，hponcfg 包括：hp-ilo, hp-health, hponcfg 3 个包。

修改密码，需要先编辑一个 xml 文件：

```

cat ilo.xml
<ribcl version="2.0">
	<login user_login="Administrator" password="mypassword">
		<user_info mode="write">
			<mod_user user_login="Administrator">
				<password value="chen"></password>
			</mod_user>
		</user_info>
	</login>
</ribcl>

```

然后执行命令：

```
hponcfg -f ilo.xml

```