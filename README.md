# Virtualization-method 

#虛擬化實現方式#

## Full Virtualization 全虛擬化 


全虛擬化就是完全
虛擬出一個主機所需要的所有環境，因為運行於其上的虛擬機來說，它感覺不到Hypervisor的存在，所以此種方案是對Guest OS的支持類型最廣泛的一種。
缺點是Hypervisor接受Guest OS kernel指令必須做動態，指令碼轉譯(Dynamic Binary Translation)成為x86架構下CPU可以解碼執行的機械碼。
e.g., 可以在指令層級如QEMU；可以在作業系統層級 如VirtualBox。

全虛擬化直接使用hypervisor分享底層硬體，也稱為
原始虛擬化技術，如圖5.2。
y hypervisor在guest VM和硬體之間用於工作協調，讓
虛擬機感覺像實體機一般地完全自由的操作，一些受
保護的指令必須由Hypervisor來捕獲和處理。
y 管理者是透過管理虛擬機Admin來控管hypervisor運
行，本結構屬VMM分類中之Hypervisor模型


顧名思義，就是底層不管3,7,21 ，虛擬機所要用的設備，全部都給它模擬。
Guest OS 是完全不知道，自己活在虛擬環境下。所以在Host OS 上的所有Guest OS , 所要做的任何I/O 作業，全部透過Host OS 的模擬器去進行。Guest OS 及Host OS 完全獨立在，不同的記憶體區塊，不會互相干擾，所以全虛擬化，理論上可以模擬運行，所有的作業系統。至於效能呢？ 運作的有點抱歉，除非不得已，小瑞不會使用這個方式。代表軟體： Virtual PC ，VM-Ware，Virtual Box，KVM等等．．．


Hypervisor 是用來建立與執行虛擬機器的部份電腦軟體。
## Para-Virtualization 半虛擬化 

嗯．．沒錯，只模擬一半的意思。也就是被模擬的Guest OS ，自己很清楚，是活在虛擬化的環境。所以很多指令，要遵守交通規則，因為它知道，自己不是這套Hardware上，唯一的OS。例如呼叫CPU ，以前是用 Ring 0 ，現在要用Ring 1，優先權稍低一點。以免多台Guest OS ，發生車禍。要做到這點，就必須修改Guest OS 的核心，把一些指令調一調。這樣的好處是，Host OS 不用模擬CPU 這個東東，負荷大幅減輕。效能就明顯優於全虛擬化。不過，不是所有OS 核心程式，都是可以改的；Linux 是Open Source 源碼公開，所以要修改很簡單，大家都可以去改；Win “暈倒" 系統呢，抱歉哦，沒有公開源碼。所以Linux 上可以執行 “半虛擬化" ， Windows 只能執行 “全虛擬化" 囉。你想要使用 “半暈倒" 系統嗎? 得等MS 公司自己改哦， 嗯．．．等花兒謝了吧…半虛擬化代表：Xen


# OS-Virtualization 作業系統層虛擬化 
