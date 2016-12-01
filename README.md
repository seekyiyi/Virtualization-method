# Virtualization-method 

#虛擬化實現方式#
![Virtualization-method](https://hackpad-attachments.s3.amazonaws.com/ncucsie.hackpad.com_QrwxkWD88gd_p.340565_1444465760010_undefined)

## Full Virtualization 全虛擬化  
虛擬機所要用的設備，全部模擬。

完全虛擬出一個主機所需要的所有環境，因為運行於其上的虛擬機來說，它感覺不到Hypervisor的存在，所以此種方案是對Guest OS的支持類型最廣泛的一種。
缺點 是Hypervisor接受Guest OS kernel指令必須做動態，指令碼轉譯(Dynamic Binary Translation)成為x86架構下CPU可以解碼執行的機械碼。

[補充:Hypervisor 是用來建立與執行虛擬機器的部份電腦軟體。]


Host OS 上的所有Guest OS , 所要做的任何I/O 作業，全部透過Host OS 的模擬器去進行。
Guest OS 及Host OS 完全獨立在，不同的記憶體區塊，不會互相干擾，所以全虛擬化。

代表軟體： Virtual PC、VM-Ware，Virtual Box、KVM。

![Virtualization-method](https://hackpad-attachments.s3.amazonaws.com/ncucsie.hackpad.com_QrwxkWD88gd_p.340565_1444488781155_undefined)

## Para-Virtualization 半虛擬化 
只模擬一半的意思。被模擬的Guest OS。

是為了改善傳統x86架構下，硬體對虛擬化無法支持，而提出的一種改進技術，實現硬體層級的虛擬化層。為了提高虛擬化的效率，需要讓虛擬機的#操作指令直接給hypervisor執行#，以減少中間轉換造成的性能損失和時間延遲。

採用 修改作業系統的核心植入 Hypercall 使得原本不能被虛擬化的指令可以透過 Hypercall Interface 像硬體提出請求，其優點是可以把 CPU、I/O 的損耗降低且理論上效能會#勝過全虛擬化#，但最大的缺點也就是必須修改作業系統核心才行，因此支援作業系統種類相對減少許多。

代表軟體例如 Xen、Microsoft Hyper-V (Parent Partition)、Citrix XenServer (Domain0) 。

旁虛擬化的modified guest OS集合了虛擬化方面的指令碼，並知道自己是在虛擬模式下運行。因為modified guest OS 運用hypercall 虛擬行程與hypervisor進行很好的合作，所以旁虛擬化的性能接近於真實系統。

![Virtualization-method](https://hackpad-attachments.s3.amazonaws.com/ncucsie.hackpad.com_QrwxkWD88gd_p.340565_1444467546409_undefined)

# OS-Virtualization 作業系統層虛擬化 

這是比較cool的虛擬化作法。也就是，它只是在原作業系統上模擬出一個行程，所有的CPU/RAM/IO 等資源，全部都共用原生的Host OS，完全沒有模擬Hardware 的負擔，所以跟在原機上執行的效能，幾乎一樣，大約只差1-3%的效能。不過限制就比較嚴格，Host OS 與Guest OS 必須使用同一個核心，所以在Linux 下只能模擬Linux ，在Windows 下，只能模擬Windows 。在這樣的模擬技術下，Guest OS 的檔案資料，基本上，你在Host OS 下可以完全看到，他只是Host OS 下的一個子目錄。
代表軟體：OpenVZ

這3種常見的虛擬化技術，效能排序為 OS 作業系統層虛擬化 > Para 半虛擬化 > Full 全虛擬化

![Virtualization-method](http://image.slidesharecdn.com/challengesandopportunitiescomputing-kuo-yi-120516090739-phpapp01/95/challenges-and-opportunities-computing-kuoyi-chen-47-728.jpg?cb=1337159465)

##補充##

Hypervisor 有哪些型態?

虛擬化的 Hypervisor 有兩種型態
- Type 1 (Bare-Metal hypervisor) 與 Type 2 (Hosted hypervisor)。Type1 又稱為 Native VM，Type2 也稱為 Hosted VM。

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
