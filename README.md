# Virtualization-method 

#虛擬化實現方式#
![Virtualization-method](https://hackpad-attachments.s3.amazonaws.com/ncucsie.hackpad.com_QrwxkWD88gd_p.340565_1444465760010_undefined)
## Full Virtualization 全虛擬化  
完全虛擬出一個主機所需要的所有環境，因為運行於其上的虛擬機來說，它感覺不到Hypervisor的存在，所以此種方案是對Guest OS的支持類型最廣泛的一種。
缺點是Hypervisor接受Guest OS kernel指令必須做動態，指令碼轉譯(Dynamic Binary Translation)成為x86架構下CPU可以解碼執行的機械碼。
e.g., 可以在指令層級如QEMU；可以在作業系統層級 如VirtualBox。

全虛擬化直接使用hypervisor分享底層硬體，也稱為原始虛擬化技術，如圖5.2。
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


是為了改善傳統x86架構下，硬體對虛擬化無法支持，

而提出的一種改進技術，實現硬體層級的虛擬化層。為了提高虛擬化的效率，需要讓虛擬機的操作指令直接給hypervisor執行，以減少中間轉換造成的性能損失和時間延遲。
在傳統x86架構中，處於kernel mode 狀態下的Kernel和Driver 使用Ring0 高特權CPU指令， 而處於usermode 狀態下APP使用Ring3低特權CPU指令，做為區隔保護系統的運作。如果guest OS一些Ring0高特權指令在低特權級模式(user mode)下運行，將會遭遇到不可預測的運行結果。
換句話說，Guest OS 所發出的低特權態部位(usermode)的敏感指令(Ring0)，這一類指令是不能自動被hypervisor (kernel mode)偵測而截獲(trap)，這是既有的環境中做不到的。因此修改Guest OS增加hypercall強迫呼叫hypervisor。
e.g., Citrix XenServer and MS Hyper‐V R2 。
缺點: 除了必須修改Guest OS以外，另外若埰用非自由軟體的作業系統做Guest OS時，除非原廠要修改，不然它是無法被修改的，例如Windows OS，因此就無法實現旁虛擬化的目標。

旁虛擬化的modified guest OS集合了虛擬化方面的指令碼，並知道自己是在虛擬模式下運行，如圖5.5。因為modified guest OS 運用hypercall 虛擬行程與hypervisor進行很好的合作，所以旁虛擬化的性能接近於真實系統。

# OS-Virtualization 作業系統層虛擬化 

這是比較cool 的虛擬化作法。也就是，它只是在原作業系統上模擬出一個行程，所有的CPU/RAM/IO 等資源，全部都共用原生的Host OS，完全沒有模擬Hardware 的負擔，所以跟在原機上執行的效能，幾乎一樣，大約只差1-3%的效能。不過限制就比較嚴格，Host OS 與Guest OS 必須使用同一個核心，所以在Linux 下只能模擬Linux ，在Windows 下，只能模擬Windows 。在這樣的模擬技術下，Guest OS 的檔案資料，基本上，你在Host OS 下可以完全看到，他只是Host OS 下的一個子目錄。
代表軟體：OpenVZ



這3種常見的虛擬化技術，效能排序為 OS > Para > Full



Hypervisor 有哪些型態?

虛擬化的 Hypervisor 有兩種型態，Type 1 (Bare-Metal hypervisor) 與 Type 2 (Hosted hypervisor)。Type1 又稱為 Native VM，Type2 也稱為 Hosted VM。

兩者有何不同?

Bare-Metal 中文譯為裸機或裸金屬，hypervisor 直接安裝於空機或新機上，直接掌控硬體資源，硬碟無須先有OS。Type 2 hypervisor 則需先有 Windows 或 Linux 才能安裝。

Type 1與Type 2 各有哪些產品?

Bare-Metal 型態：VMware ESX / ESXi、Microsoft Hyper-V、Citrix Xen Server、RedHat KVM、Oracle Virtual Iron 等。

Hosted 型態：VMware Wrokstation / Fusion、Microsoft Virtual PC / Server、Sun VirtualBox、Parallels Desktop 等。

所以 Bare-Metal hypervisor 是半虛擬化產品，而 Hosted hypervisor 是全虛擬化產品?

錯。半虛擬化與全虛擬化指的是廠商在解決 x86 CPU 特權等級，運用不同的技術，解決敏感指令不能被虛擬化的問題，與 Bare-Metal 或 Hosted 安裝方式無關，這是一般大眾的誤解。

那麼 Bare-Metal hypervisor 是企業級用途，而 Hosted hypervisor 是個人用途?

並不盡然，但目前要這麼說也無不可。Bare-Metal hypervisor 在效能、穩定度、安全性確實優於 Hosted hypervisor，所以企業應用的虛擬化解決方案皆為 Bare-Metal 形式。但是個人用途的虛擬化也有 Bare-Metal 形式的 hypervisor 存在，有人稱為 Bare-Metal desktop hypervisor。

Bare-Metal desktop hypervisor 的廠商或產品有哪些?

Virtual Computer、Neocleus (現已被 Intel 收購)、XenClient。 
