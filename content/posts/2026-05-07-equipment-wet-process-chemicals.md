---
title: "先進封裝的濕製程與化學藥液：不只是把晶圓洗乾淨"
date: "2026-05-07T22:20:00+08:00"
slug: "2026-05-07-equipment-wet-process-chemicals"
categories: ["equipment", "materials"]
tags: ["濕製程", "先進封裝", "CoWoS", "FOPLP", "化學藥液", "設備"]
draft: false
newsletter_candidate: true
podcast_candidate: true
source_disclosure: "one_representative_first_party_url_per_company"
---

<p><strong>本文是資訊整理與產業知識分享，不構成任何投資建議。</strong></p>

<p>先進封裝討論常從 CoWoS、chiplet、HBM、RDL 或封裝產能開始，但真正走進製程現場，會反覆遇到另一個不容易被看見的環節：濕製程與化學藥液。它不只是「把晶圓洗乾淨」，而是把清洗、蝕刻、去氧化、去殘膠、乾燥、表面狀態控制串在一起，讓後面的鍵合、填膠、重工與可靠度測試能夠接上。</p>

<figure>
  <img src="/ai-intel/images/posts/wet-process-equipment.png" alt="濕製程設備示意">
  <figcaption>濕製程設備可以理解為把化學藥液、沖洗、乾燥與參數控制整合在同一套製程平台。圖中以簡化方式呈現設備如何處理不同表面狀態，並非實際設備剖面。</figcaption>
</figure>

<p>傳統半導體新聞裡，濕製程常被簡化成清洗或蝕刻。這樣說不算錯，但放到先進封裝就太簡化。以 CoWoS 類型的封裝來看，濕製程可以拆成多個階段：pre-clean、TSV / RDL clean、micro-bump clean、pre-bond clean、flux clean、post / rework clean。每一段看起來都在「清洗」，但目的並不相同。</p>

<ul>
  <li><strong>Pre-clean：</strong>先把表面污染物、微粒與有機殘留降下來，讓後面製程不要一開始就帶著問題前進。</li>
  <li><strong>TSV / RDL clean：</strong>處理銅、介電層、polymer residue 等多種材料共存的表面，重點是選擇性與低損傷。</li>
  <li><strong>Micro-bump clean：</strong>微凸塊 pitch 越來越小後，氧化與殘留都可能放大成接合風險。</li>
  <li><strong>Pre-bond clean：</strong>鍵合前要處理氧化層、微粒與表面活化，hybrid bonding 對這一段特別敏感。</li>
  <li><strong>Flux clean：</strong>把助焊劑殘留移除，避免後續 underfill void 或長期可靠度問題。</li>
  <li><strong>Post / rework clean：</strong>處理返修、殘膠與最終清洗，關係到產品能不能穩定交付。</li>
</ul>

<p>換句話說，濕製程的價值不只是單點設備規格，而是把材料表面、化學反應、流場、乾燥、particle control 和客戶 recipe 串起來。先進封裝越往高密度、小 pitch、多材料堆疊與大尺寸面板前進，這些看似細節的清洗與去除能力就越容易成為良率瓶頸。</p>

<h2>CoWoS、InFO、FOPLP 放大的風險不同</h2>

<p>把 CoWoS、InFO、FOPLP 分開對理解上很有幫助，因為不同封裝架構放大的是不同風險。</p>

<p><strong>CoWoS</strong> 比較接近高階晶圓級與 2.5D 封裝思維，濕製程常碰到 TSV、RDL、Cu seed、pre-bond oxide removal 等問題。它的關鍵不只是洗得乾淨，而是在銅、介電層、氧化層與 polymer 共存時，還能控制選擇性與表面狀態。</p>

<p><strong>InFO</strong> 的有機材料比例較高，RDL、PI、EMC 與載板後段清洗會讓殘膠、粒子移除、表面能控制變得重要。這裡的挑戰不只是化學藥液有效，還要避免對有機材料造成不必要的損傷。</p>

<p><strong>FOPLP</strong> 則把問題放大到 panel-level。尺寸變大後，warpage、流場不均、深縫滲透、乾燥均勻性都會變成新問題。原本在 wafer 上可行的 recipe，不一定能直接搬到 panel 上使用。</p>

<p>因此，同樣叫濕製程，在 CoWoS、InFO、FOPLP 裡面代表的工程風險並不完全相同。CoWoS 更強調多材料界面與高精度清洗，InFO 更強調有機材料與殘膠控制，FOPLP 則更強調大面積均勻性與滲透能力。</p>

<h2>設備與藥液需要一起看</h2>

<p>弘塑是濕製程設備商，添鴻科技則是弘塑旗下化學品材料商，這個組合剛好可以說明：濕製程不是買一台設備就結束，而是設備、藥水、參數和客戶製程一起調整。</p>

<p>一套濕製程設備可能要處理清洗、蝕刻、去膠、乾燥、megasonic、溫度、濃度、流速與污染控制。化學藥液則要在去除目標材料的同時，避免傷到不該被攻擊的表面。兩者若沒有一起調整，就容易出現「看起來洗掉了，但後面 bonding / underfill / reliability 出問題」的情況。</p>

<figure>
  <img src="/ai-intel/images/posts/wet-process-chemicals.png" alt="濕製程化學藥液功能示意">
  <figcaption>同一種封裝結構，在不同製程階段會需要不同化學藥液功能。這張圖用相同結構示意清洗、去氧化、去殘膠與選擇性移除的差異，重點是幫助理解化學藥液功能，不代表實際材料堆疊。</figcaption>
</figure>

<h2>六家公司代表不同位置</h2>

<p>這次放在同一篇文章中的六家公司，不是同一種商業模式，也不是同一個製程位置，而是共同構成濕製程與化學藥液這條鏈的不同節點。</p>

<table>
  <thead>
    <tr>
      <th>公司</th>
      <th>在本題中的角色</th>
      <th>可以觀察的重點</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>弘塑</td>
      <td>濕製程設備</td>
      <td>先進封裝清洗、蝕刻、去阻劑、乾燥與客製 recipe 的整合能力。</td>
    </tr>
    <tr>
      <td>辛耘</td>
      <td>濕製程設備、panel-level 相關設備</td>
      <td>WSE Panel、PolerPanel、暫時貼合 / 解貼合與 panel-level packaging 的延伸性。</td>
    </tr>
    <tr>
      <td>添鴻科技</td>
      <td>蝕刻液、清洗液與電子級化學品</td>
      <td>說明設備體系中的化學品端，與濕製程設備搭配解決製程問題。</td>
    </tr>
    <tr>
      <td>勝一</td>
      <td>電子級溶劑</td>
      <td>半導體清洗、光阻與化學品供應鏈中的基礎材料角色。</td>
    </tr>
    <tr>
      <td>三福</td>
      <td>精密化學品</td>
      <td>電子級化學品、氣體、特殊化學品與本土供應能力。</td>
    </tr>
    <tr>
      <td>達興</td>
      <td>temporary bonding / de-bonding 與 remover</td>
      <td>在先進封裝、RDL、晶圓薄化或暫時貼合製程中，材料與移除能力的重要性。</td>
    </tr>
  </tbody>
</table>

<h2>濕製程風險分成三層</h2>

<p>先進封裝的濕製程風險大致分成三層。</p>

<p>第一層是「能不能做」。pre-clean、TSV / RDL clean、micro-bump clean 如果處理不好，製程可能在前段就埋下問題，後面再怎麼補救都很困難。</p>

<p>第二層是「能不能量產」。到了 pre-bond clean 與 flux clean，問題不只在單片樣品是否成功，而是量產時能不能維持一致性。FOPLP 這類大尺寸 panel 更會放大流場、滲透、乾燥與均勻性的挑戰。</p>

<p>第三層是「能不能交付」。post / rework clean、殘膠移除、underfill 前後的清潔狀態，可能不會馬上造成外觀看得見的問題，卻會在熱循環、可靠度測試或長期使用中浮現。</p>

<h2>為什麼值得追蹤</h2>

<p>AI/HPC 晶片把封裝往更大尺寸、更高密度與更複雜材料組合推進，濕製程也跟著從後段支援角色，變成先進封裝良率的一部分。這條鏈不一定每次都出現在新聞標題，但會出現在設備規格、材料導入、客戶認證與產能擴充的細節裡。</p>

<p>從這個角度看，弘塑、辛耘、添鴻科技、勝一、三福與達興放在同一篇文章中是合理的，但它們代表的是不同位置：有人做設備平台，有人做化學品，有人做溶劑，有人做 temporary bonding / remover。未來追蹤這條鏈，不應只看單一公司新聞，而要看先進封裝往 2.5D / 3D、panel-level、RDL、hybrid bonding 前進時，哪一段清洗與去除風險被放大。</p>

<h2>代表來源</h2>

<ul>
  <li>弘塑：<a href="https://www.gptc.com.tw/product-detail/UFO-300C-Series/">UFO-300C Series</a></li>
  <li>辛耘：<a href="https://www.scientech.com.tw/zh-hant/Products/Scientech/WSEPanel">WSE Panel</a></li>
  <li>添鴻科技：<a href="https://en.chemleader.com.tw/autopage_detail/6/cu-etchant">Cu Etchant</a></li>
  <li>勝一：<a href="https://www.shinychem.com.tw/index.php?id=2&lang=cht&option=product&task=showlist">半導體應用產品頁</a></li>
  <li>三福：<a href="https://www.sfchem.com.tw/">官方網站</a></li>
  <li>達興：<a href="https://www.daxinmat.com/?c=311&lang=en-US&sn=844">Semiconductor Materials</a></li>
</ul>

