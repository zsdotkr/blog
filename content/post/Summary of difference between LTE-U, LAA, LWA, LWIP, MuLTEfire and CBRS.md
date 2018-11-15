---
title: Summary of difference between LTE-U, LAA, LWA, LWIP, MuLTEfire and CBRS
date: 2018-10-01
categories: [tech]
tags: [lte]
draft: false
version: 1.0
---



This article is written to compare briefly characteristics of six different kinds of LTE technologies using an unlicensed band, primarily for WiFi services. 

<!--more-->

#### Reference Standards 

- LTE-U : 3GPP R12 modification by LTE-U Forum
- LAA / eLAA : 3GPP R13 (LAA) / R14 (eLAA) 
- LWA / eLWA : 3GPP R13 (LWA) / R14 (eLWA) 
- LWIP : 3GPP R13 
- MuLTEfire : 3GPP R13 & R14 modification mainly at the radio layer by MuLTEfire Forum


#### Frequency for Unlicensed Band 

- LTE-U : 5GHz Only  
- LAA / eLAA : 5GHz Only  
- LWA / eLWA : Any WiFi band including 60GHz (802.11ax / WiFi6)  
- LWIP : Any WiFi band including 60GHz 
- MuLTEfire : mainly 5GHz band for data service and sub-1GHz ISM band for a IoT service



#### Serving Nodes Configuration  

- LTE-U : eNB which serve both LTE and unlicensed band
- LAA / eLAA : eNB which serve both LTE and unlicensed band
- LWA / eLWA : eNB serve LTE band + WT serve both unlicensed band and Wifi 

	> *3GPP aims to use existing WiFi AP as their WT (Wireless Terminal), only with a software upgrade to serve both WiFi and LWA/eLWA connectivity. 3GPP already adopted similar concept at their TWAN architecture. eNB does overall management for WTs connected to the eNB.*
- LWIP :  eNB serve LTE band + Commercial WiFi AP/AC + LWIP capable UE

	> *3GPP aims not to touch anything of WiFi AP/AC, touch UE instead. 3GPP already adopted similar concept at their ePDG architecture. eNB does not manage WiFi AP/AC at all, instead, interacts with UE directly.*
- MuLTEfire : MuLTEfire capable eNB and UE

	> *MuLTEfire Forum's intent : Building LTE only with unlicensed bands without the support of a licensed band while keeping interoperability with existing LTE core entities (SGW, PGW, MME, HSS, PCRF and others)*

#### Radio Channel Aggregation

- LTE-U : use CA (carrier aggregation) between LTE and unlicensed band

	> *An eNB have wireless physical layers (RLC & MAC) for both LTE and unlicensed, use LTE band as a primary link (signal+ data) and unlicensed band as a secondary link (data without signal)* 

- LAA / eLAA : use CA between LTE and unlicensed band 

	> *Same as LTE-U. CA is possible* 

- LWA / eLWA : use DC (dual connectivity) between LTE and unlicensed band 

	> An eNB does not have a physical layer for unlicensed band, owned by WT. 
	>
	> DC is designed for these use-cases: when need to aggregate wholly different wireless technologies together; when those different RLC/MACs could not be synchronized - connected through non-ideal path (e.g., because of network/protocol delay)  

- LWIP : use DC between LTE and unlicensed band

	> *Same as LWA/eLWA, Only DC is possible*

- MuLTEfire : Not clear

	> *Both CA and DC could be in scope because MuLTEfire is based on R-13 ~ 14. But MuLTEfire has no anchoring band which should be stable and available at any time which LTE have.*   



More about CA and DC, 

- in CA : Single IP packet can be delivered through **both LTE and unlicensed band** after fragmented at wireless radio layer (RLC layer does this); In an example, if eNB assigns 5Mbps at LTE and 10Mbps at unlicensed band to UE, user can experience 15Mbps high-quality youtube single video stream. 
- in DC : Single IP packet should be delivered through **one of LTE or unlicensed band**; In an example, if eNB assigns 5Mbps at LTE and 10Mbps at unlicensed band to UE, user can experience 5Mbps low-quality or 10Mbps mid-quality youtube video stream and can use remaining bandwidth (10 or 5Mbps) for another application at the same time.
- [Fig. 4 in this article](https://www.netmanias.com/en/post/blog/7388/c-ran-fronthaul-kt-korea-lte-lte-h-lte-u-mwc-2015-samsung/netmanias-interview-with-kt-at-mwc-2015-kt-s-demonstrations-of-lte-h-and-lte-u)  will be helpful to understand the physical layer configuration for CA (single PDCP/RLC + multiple MAC)
- See [this article](http://www.sharetechnote.com/html/Handbook_LTE_LWA.html) for DC (single PDCP + multiple RLC/MAC); Fig. 2 in the previous article explaining LTE-H also be helpful to how DC works. 



#### Service Direction and Air Frame Format for Unlicensed Band  

- LTE-U : DL Only. use 3GPP Frame Structure Type 1 (FDD) + CSAT  
- LAA / eLAA : DL Only (LAA), DL & UL (eLAA). use 3GPP Frame Structure Type 3 

	> *LTE Frame Structure Type 3 : LTE TDD-like frame format to use a single unlicensed frequency for both uplink and downlink. also, have LBT capability to meet national regulations to use unlicensed band.* 

- LWA / eLWA : DL Only (LWA), DL & UL (eLWA). use 802.11 WiFi frame 

	> *GTP-U tunnel is used between eNB & WT, [Fig.7 in this article](https://www.netmanias.com/en/?m=view&id=reports&no=8532) will be helpful to understand how it works.* 

- LWIP : DL & UL, use 802.11 WiFi frame

	> *IPSec tunnel is used between eNB and UE to transfer user data and signaling. WiFi AP is used only to deliver IPSec packets.*   

- MuLTEfire : DL & UL, use their proprietary frame format

	> *They extend "3GPP frame structure type 3" to use solely the unlicensed band without the help from a licensed band. And also adopt a different kind of modulation method called "RB interlace" to extend coverage instead of SC-FDMA which is used for LTE uplink modulation method for frame structure type 1 to 3. Need more details? See [here](https://www.MuLTEfire.org/wp-content/uploads/2016/10/MuLTEfire_Radio-Link.pdf)*



see 3GPP TS 36.300 chapter 5 for the definition of frame structure type 1 ~ 3. and [this](https://ofinno.com/technology/enhanced-laa/) will also be helpful to understand the differences.  



#### CBRS (Citizen Broadband Radio Service) 

CBRS is sometimes called as one of the unlicensed LTE technologies but has a different purpose. Precisely speaking, LAA ~ LWIP shares and uses an unlicensed band **with other wireless technologies**, mainly WiFi. But CBRS shares a licensed band, called as CBRS band, **with other eNBs owned by another LTE operator**; other wireless technologies such as **WiFi cannot use CBRS band**.

> In 2017, the FCC completed a process to establish rules for commercial use of this band. Commercializing this band enables **Service Providers/Wireless Operators to use it without acquiring frequency licenses**. CBRS frequency spectrum shall help 4G/5G mobile networks deployment quicker and easier. CBRS band is also referred as **3.5 GHz band**.  The main users of the 3.5 GHz band are the US military services: The US Air Force, Army, Coast Guard, Marines and Navy. The US government’s intent is to make this band available for commercial flexible wireless broadband use, leading to improved broadband access and performance for consumers. - refer [this article](http://www.techplayon.com/citizens-broadband-radio-service-cbrs-frequency-spectrum-operation-modes-and-its-applications/)



#### Why mobile operators so eager to use unlicensed?

Because... acquiring a licensed frequency band for a LTE service is **very EXPENSIVE**. 

> In a recent example, 5G spectrum auction, 3.5GHz CBRS band and 28GHz mmWave band, at Korea ended on last June 2018. Three major mobile operators in Korea, SKT, KT, LGU+, acquire licenses to use CBRS frequency band, from 3.42GHz to 3.7GHz, 280MHz width, for 10 years after paying a total of 2.77 billion USD (2.99 trillion KRW). Almost **9.9 million USD per 1MHz.**  - refer [this news](http://english.yonhapnews.co.kr/search1/2603000000.html?cid=AEN20180618010500320)



#### Trials & Deployments  

- See this [GSA report, Jan. 2018](https://www.sata-sec.net/downloads/GSA/180117-GSA-Unlicensed-spectrum-report-Jan-2018.pdf), which drive me to write this article 

	> *"As of late 2018, all trial and commercial deployments of pre-standards LTE Forum LTE-U technology have been shut down or upgraded to the 3GPP Standard release 13 LTE-LAA" - wikipedia*

#### Abbreviations

- LAA / eLAA : License Assisted Access / enhanced LAA
- LTE-U : LTE Unlicensed 
- LWA / eLWA : LTE Wireless LAN Aggregation / enhanced LWA 
- LBT : Listen Before Talk 
- CSAT : Carrier Sensing Adaptive Transmission 
- WT : WLAN Terminal (normally WiFi AP which interacts with eNB) 
- DL : Downlink, packets flowing from outside of LTE/EPC to UE  
- UL : Uplink, packets flowing from UE to outside of LTE/EPC
- PDCP : Packet Data Convergence Protocol, the lowest handler for IP frame, perform encryption/decryption for radio interface, and compression optionally called IPCOMP to minimize overhead during air transmission.
- RLC : Radio Link Control, topmost handler for an air transmission, perform fragment/reassembly/retransmission at air interface 

---

*Please, comments if there is anything wrong or need to update.*   

*Written by [zsdotkr@gmail.com](mailto:zsdotkr@gmail.com) with CC license : [CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)* 

