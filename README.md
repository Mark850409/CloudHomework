# 雲端運算 作業操作步驟

## 簡介

透過負載平衡的方式建立虛擬機，並建立映像檔，爾後以此映像檔快速建至環境

## 目錄

- [雲端運算 作業操作步驟](#雲端運算-作業操作步驟)
  - [簡介](#簡介)
  - [目錄](#目錄)
  - [2023/11/13 nginx無法執行錯誤解決方案](#20231113-nginx無法執行錯誤解決方案)
  - [作業1：建立 Azure 虛擬機器1](#作業1建立-azure-虛擬機器1)
    - [STEP1：建立資源群組](#step1建立資源群組)
    - [STEP2：輸入以下內容](#step2輸入以下內容)
    - [STEP3：點選建立](#step3點選建立)
    - [STEP4：建立虛擬機器，請依圖片紅框處操作](#step4建立虛擬機器請依圖片紅框處操作)
    - [STEP5：點選建立](#step5點選建立)
    - [STEP6：查看自己主機的公用IP，用cloud shell或是cmd連入虛擬機](#step6查看自己主機的公用ip用cloud-shell或是cmd連入虛擬機)
    - [STEP7：完成畫面如下](#step7完成畫面如下)
  - [作業2：建立 Azure 虛擬機器1](#作業2建立-azure-虛擬機器1)
    - [STEP1：選取資源群組，設定虛擬機器名稱、區域、使用者帳密、ports(如紅框處設定)](#step1選取資源群組設定虛擬機器名稱區域使用者帳密ports如紅框處設定)
    - [STEP2：建立負載平衡器](#step2建立負載平衡器)
    - [STEP3：建立負載平衡輸入NAT規則](#step3建立負載平衡輸入nat規則)
    - [STEP4：進入剛建好負載平衡器的VM，點選擷取](#step4進入剛建好負載平衡器的vm點選擷取)
    - [STEP5：建立虛擬機器映象名稱](#step5建立虛擬機器映象名稱)
    - [STEP6：建立虛擬機器版本號碼](#step6建立虛擬機器版本號碼)
    - [STEP7：建立標籤，如紅框處設定](#step7建立標籤如紅框處設定)
    - [STEP8：完成畫面如下](#step8完成畫面如下)
  - [作業3：以映像檔建立虛擬機器1擴展集](#作業3以映像檔建立虛擬機器1擴展集)
    - [STEP1：進入資源群組，點選建立好的ACG，再點選紅框處](#step1進入資源群組點選建立好的acg再點選紅框處)
    - [STEP2：點選紅框處(版本號碼)](#step2點選紅框處版本號碼)
    - [STEP3：點選建立VM](#step3點選建立vm)
    - [STEP4：選擇資源群組、設定虛擬機名稱](#step4選擇資源群組設定虛擬機名稱)
    - [STEP5：建立使用者名稱、密碼，選擇80\&22 port](#step5建立使用者名稱密碼選擇8022-port)
    - [STEP6：選擇原先建立好的負載平衡設定](#step6選擇原先建立好的負載平衡設定)
    - [STEP7：建立標籤，如紅框處設定](#step7建立標籤如紅框處設定-1)
    - [STEP8：完成畫面如下](#step8完成畫面如下-1)

## 2023/11/13 nginx無法執行錯誤解決方案

> Q1：有同學建立第三台虛擬機發生以下錯誤，如下圖，可參考以下步驟解決

![無法連線](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/無法連線.png)


STEP1：在建立映像時，"自動在建立映像後刪除此虛擬機器"選項最好打勾，不然在啟動時有可能會有問題
![image-20231113140020832](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231113140020832.png)


STEP2：進入lb後端集區，查看兩台VM是否都被加入後端集區，如果沒有請手動加入
![image-20231113135215068](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231113135215068.png)


STEP3：請檢查下方操作步驟是否還有其他遺漏的部份

---

> Q2：原本設定的NAT規則明明沒更改，但是在建立第二台虛擬機後卻無法連線了


![image-20231113135359803](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231113135359803.png)


STEP1：進入NAT輸入規則，將目標更改為VM2，網路IP位址也請除新選擇
![image-20231113135055046](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231113135055046.png)


STEP2：更改完後，請輸入以下指令進行測試

```
ssh [你的公用IP] -l [你設定的帳號] -p [你設定的port]
```
![image-20231113135523787](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231113135523787.png)


STEP3：登入成功後，可以用以下指令驗證nginx伺服器狀態
```
systemctl status nginx
```
![image-20231113135643440](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231113135643440.png)



## 作業1：建立 Azure 虛擬機器1

### STEP1：建立資源群組

![image-20231112151208461](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112151208461.png)

### STEP2：輸入以下內容
```
unit vm
purpose course
scenario 3
```
![image-20231112151407295](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112151407295.png)

### STEP3：點選建立
![image-20231112151436784](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112151436784.png)

### STEP4：建立虛擬機器，請依圖片紅框處操作
![image-20231112152454710](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112152454710.png)

![image-20231112152539394](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112152539394.png)

![image-20231112152613563](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112152613563.png)

![image-20231112153137297](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112153137297.png)

### STEP5：點選建立
![image-20231112153211438](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112153211438.png)

### STEP6：查看自己主機的公用IP，用cloud shell或是cmd連入虛擬機
```shell
ssh [你的公用IP] -l [你設定的帳號]
```
![image-20231112153943910](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112153943910.png)

```shell
sudo apt-get update
```

![image-20231112154137482](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112154137482.png)

```shell
sudo apt-get install nginx -y 
```
![image-20231112154357367](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112154357367.png)

### STEP7：完成畫面如下

![image-20231112154711596](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112154711596.png)

![image-20231112154741729](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112154741729.png)



## 作業2：建立 Azure 虛擬機器1


### STEP1：選取資源群組，設定虛擬機器名稱、區域、使用者帳密、ports(如紅框處設定)
![image-20231112160157517](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112160157517.png)

![image-20231112160231900](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112160231900.png)

![image-20231112160510562](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112160510562.png)


### STEP2：建立負載平衡器
![image-20231112160735778](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112160735778.png)



![image-20231112161441193](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112161441193.png)

![image-20231112161747558](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112161747558.png)

![image-20231112162846280](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112162846280.png)

### STEP3：建立負載平衡輸入NAT規則
![image-20231112163012188](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112163012188.png)

![image-20231112163527366](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112163527366.png)

![image-20231112163838051](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112163838051.png)

### STEP4：進入剛建好負載平衡器的VM，點選擷取
![image-20231112164520837](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112164520837.png)

![image-20231112164758511](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112164758511.png)

### STEP5：建立虛擬機器映象名稱
![image-20231112165044669](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112165044669.png)

### STEP6：建立虛擬機器版本號碼
![image-20231112165306877](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112165306877.png)

### STEP7：建立標籤，如紅框處設定
![image-20231112165419271](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112165419271.png)

![image-20231112165604091](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112165604091.png)


### STEP8：完成畫面如下
![image-20231112170931322](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112170931322.png)

## 作業3：以映像檔建立虛擬機器1擴展集

### STEP1：進入資源群組，點選建立好的ACG，再點選紅框處
![image-20231112171528191](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112171528191.png)

### STEP2：點選紅框處(版本號碼)
![image-20231112171556622](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112171556622.png)


### STEP3：點選建立VM
![image-20231112171629894](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112171629894.png)

### STEP4：選擇資源群組、設定虛擬機名稱
![image-20231112172131794](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112172131794.png)

### STEP5：建立使用者名稱、密碼，選擇80&22 port
![image-20231112172219715](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112172219715.png)

![image-20231112172333848](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112172333848.png)

### STEP6：選擇原先建立好的負載平衡設定
![image-20231112172447734](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112172447734.png)

### STEP7：建立標籤，如紅框處設定
![image-20231112172624324](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112172624324.png)


![image-20231112172714765](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112172714765.png)

### STEP8：完成畫面如下
![image-20231112174445930](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112174445930.png)

![image-20231112174336897](https://raw.githubusercontent.com/Mark850409/UploadGithubImage/master/image-20231112174336897.png)