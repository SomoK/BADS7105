# Voice of Customers

Load libraries and data<a class="anchor-link" href="#Load-libraries-and-data">&#182;</a></h2>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[1]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">pythainlp</span>
</pre>

    




<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s1">&#39;CustomerReviews.csv&#39;</span><span class="p">)</span>
</pre>

    




<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

    



<div class="output_wrapper">
<div class="output">


<div class="output_area">
    
<div class="prompt input_prompt">Out&nbsp;[3]:



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Review ID</th>
      <th>Restaurant_ID</th>
      <th>Restaurant</th>
      <th>User</th>
      <th>Headline</th>
      <th>Review</th>
      <th>Rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>ployynp</td>
      <td>บุฟเฟ่ต์ชาบูและพิซซ่าไม่อั้นในราคา 199 บาท เน้...</td>
      <td>หลังจากที่เคยลองสาขายูเนี่ยนมอลล์ไป รอบนี้มาที...</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>27a91236fe5e4559a4f097c97a480781</td>
      <td>ร้านบุฟเฟ่ต์ ราคามิตรภาพ อยู่ชั้น4 ติดโรงหนัง ...</td>
      <td>ร้านบุฟเฟ่ต์ที่มีโปรโมชั่นหัวละ199บาท ไม่รวมน้...</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>0b81d251e4db486f9bcdba73b374ed99</td>
      <td>ของหลากหลาย ปนๆ งงๆ นิดหน่อย</td>
      <td>เคยรู้จักร้านนี้จากที่ union mall ไม่เคยได้ลอง...</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>40e0e087f3914fd49a8933b5a29936ca</td>
      <td>อร่อยมากค่ะ คุ้มค่าสมราคา บุฟเฟ่หมูผักต่างๆ รว...</td>
      <td>อร่อยมากค่ะ คุ้มค่าสมราคา บุฟเฟ่หมูผักต่างๆ รว...</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>41841cb99ea243a3a8d4b006e946c586</td>
      <td>แม้จะแปลกบ้าง แต่ก็ถือว่าอยู่ในเกณฑ์ที่ดี มีอา...</td>
      <td>ก็ตามที่เขียนเลยครับ ว่า ถ้าจะจ่ายในราคา 199 บ...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>









<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Tokenize-Words">Tokenize Words<a class="anchor-link" href="#Tokenize-Words">&#182;</a></h2>



<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[4]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">stopwords</span> <span class="o">=</span> <span class="nb">list</span><span class="p">(</span><span class="n">pythainlp</span><span class="o">.</span><span class="n">corpus</span><span class="o">.</span><span class="n">thai_stopwords</span><span class="p">())</span>
<span class="n">removed_words</span> <span class="o">=</span> <span class="p">[</span><span class="s1">&#39; &#39;</span><span class="p">,</span> <span class="s1">&#39;  &#39;</span><span class="p">,</span> <span class="s1">&#39;</span><span class="se">\n</span><span class="s1">&#39;</span><span class="p">,</span> <span class="s1">&#39;ร้าน&#39;</span><span class="p">,</span> <span class="s1">&#39;(&#39;</span><span class="p">,</span> <span class="s1">&#39;)&#39;</span> <span class="p">,</span> <span class="s1">&#39;           &#39;</span><span class="p">]</span>
<span class="n">screening_words</span> <span class="o">=</span> <span class="n">stopwords</span> <span class="o">+</span> <span class="n">removed_words</span>

<span class="k">def</span> <span class="nf">tokenize_with_space</span><span class="p">(</span><span class="n">sentence</span><span class="p">):</span>
  <span class="n">merged</span> <span class="o">=</span> <span class="s1">&#39;&#39;</span>
  <span class="n">words</span> <span class="o">=</span> <span class="n">pythainlp</span><span class="o">.</span><span class="n">word_tokenize</span><span class="p">(</span><span class="nb">str</span><span class="p">(</span><span class="n">sentence</span><span class="p">),</span> <span class="n">engine</span><span class="o">=</span><span class="s1">&#39;newmm&#39;</span><span class="p">)</span>
  <span class="k">for</span> <span class="n">word</span> <span class="ow">in</span> <span class="n">words</span><span class="p">:</span>
    <span class="k">if</span> <span class="n">word</span> <span class="ow">not</span> <span class="ow">in</span> <span class="n">screening_words</span><span class="p">:</span>
      <span class="n">merged</span> <span class="o">=</span> <span class="n">merged</span> <span class="o">+</span> <span class="s1">&#39;,&#39;</span> <span class="o">+</span> <span class="n">word</span>
  <span class="k">return</span> <span class="n">merged</span><span class="p">[</span><span class="mi">1</span><span class="p">:]</span>
</pre>

    




<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[5]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df</span><span class="p">[</span><span class="s1">&#39;Review_tokenized&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">&#39;Review&#39;</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">tokenize_with_space</span><span class="p">(</span><span class="n">x</span><span class="p">))</span>
</pre>

    




<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[6]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

    



<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt input_prompt">Out&nbsp;[6]:



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Review ID</th>
      <th>Restaurant_ID</th>
      <th>Restaurant</th>
      <th>User</th>
      <th>Headline</th>
      <th>Review</th>
      <th>Rating</th>
      <th>Review_tokenized</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>ployynp</td>
      <td>บุฟเฟ่ต์ชาบูและพิซซ่าไม่อั้นในราคา 199 บาท เน้...</td>
      <td>หลังจากที่เคยลองสาขายูเนี่ยนมอลล์ไป รอบนี้มาที...</td>
      <td>4.0</td>
      <td>หลังจากที่,ลอง,สาขา,ยู,นม,อลล์,รอบ,สาขา,เดอะ,ม...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>27a91236fe5e4559a4f097c97a480781</td>
      <td>ร้านบุฟเฟ่ต์ ราคามิตรภาพ อยู่ชั้น4 ติดโรงหนัง ...</td>
      <td>ร้านบุฟเฟ่ต์ที่มีโปรโมชั่นหัวละ199บาท ไม่รวมน้...</td>
      <td>4.0</td>
      <td>บุฟเฟ่ต์,โปรโมชั่น,หัว,199,บาท,น้ำ,VAT,ทาน,ธรร...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>0b81d251e4db486f9bcdba73b374ed99</td>
      <td>ของหลากหลาย ปนๆ งงๆ นิดหน่อย</td>
      <td>เคยรู้จักร้านนี้จากที่ union mall ไม่เคยได้ลอง...</td>
      <td>3.0</td>
      <td>รู้จัก,union,mall,ลอง,กิน,จำได้,ขึ้นใจ,ชื่อ,😆,...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>40e0e087f3914fd49a8933b5a29936ca</td>
      <td>อร่อยมากค่ะ คุ้มค่าสมราคา บุฟเฟ่หมูผักต่างๆ รว...</td>
      <td>อร่อยมากค่ะ คุ้มค่าสมราคา บุฟเฟ่หมูผักต่างๆ รว...</td>
      <td>5.0</td>
      <td>อร่อย,คุ้มค่า,สมราคา,บุ,ฟเฟ่,หมู,ผัก,น้ำ,จบ,25...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>436045MJ-ข้าน้อยขอชาบู</td>
      <td>ข้าน้อยขอชาบู</td>
      <td>41841cb99ea243a3a8d4b006e946c586</td>
      <td>แม้จะแปลกบ้าง แต่ก็ถือว่าอยู่ในเกณฑ์ที่ดี มีอา...</td>
      <td>ก็ตามที่เขียนเลยครับ ว่า ถ้าจะจ่ายในราคา 199 บ...</td>
      <td>NaN</td>
      <td>จ่าย,ราคา,199,บาท,จ่าย,เงินสด,ราคา,น้ำ,VAT,7,%...</td>
    </tr>
  </tbody>
</table>









<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Topic">Topic<a class="anchor-link" href="#Topic">&#182;</a></h2>



<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[7]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">positive_keys</span> <span class="o">=</span> <span class="p">[</span><span class="s1">&#39;ดี&#39;</span><span class="p">,</span> <span class="s1">&#39;ฟรี&#39;</span><span class="p">,</span> <span class="s1">&#39;อร่อย&#39;</span><span class="p">]</span>
<span class="n">df</span><span class="p">[</span><span class="s1">&#39;Compliments&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">&#39;Review_tokenized&#39;</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="p">[</span><span class="n">x</span><span class="o">.</span><span class="n">split</span><span class="p">(</span><span class="s1">&#39;,&#39;</span><span class="p">)[</span><span class="n">i</span><span class="o">-</span><span class="mi">1</span><span class="p">]</span><span class="o">+</span><span class="n">w</span> <span class="k">for</span> <span class="n">i</span><span class="p">,</span><span class="n">w</span> <span class="ow">in</span> <span class="nb">enumerate</span><span class="p">(</span><span class="n">x</span><span class="o">.</span><span class="n">split</span><span class="p">(</span><span class="s1">&#39;,&#39;</span><span class="p">))</span> <span class="k">if</span> <span class="n">w</span> <span class="ow">in</span> <span class="n">positive_keys</span><span class="p">])</span>
<span class="n">negative_keys</span> <span class="o">=</span> <span class="p">[</span><span class="s1">&#39;ไม่&#39;</span><span class="p">]</span>
<span class="n">df</span><span class="p">[</span><span class="s1">&#39;Criticisms&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">&#39;Review_tokenized&#39;</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="p">[</span><span class="n">w</span><span class="o">+</span><span class="n">x</span><span class="o">.</span><span class="n">split</span><span class="p">(</span><span class="s1">&#39;,&#39;</span><span class="p">)[</span><span class="n">i</span><span class="o">+</span><span class="mi">1</span><span class="p">:</span><span class="n">i</span><span class="o">+</span><span class="mi">3</span><span class="p">]</span> <span class="k">for</span> <span class="n">i</span><span class="p">,</span><span class="n">w</span> <span class="ow">in</span> <span class="nb">enumerate</span><span class="p">(</span><span class="n">x</span><span class="o">.</span><span class="n">split</span><span class="p">(</span><span class="s1">&#39;,&#39;</span><span class="p">))</span> <span class="k">if</span> <span class="n">w</span> <span class="ow">in</span> <span class="n">negative_keys</span><span class="p">])</span>
</pre>

    




<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[8]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df</span><span class="p">[[</span><span class="s1">&#39;Restaurant&#39;</span><span class="p">,</span> <span class="s1">&#39;Review&#39;</span><span class="p">,</span> <span class="s1">&#39;Review_tokenized&#39;</span><span class="p">,</span> <span class="s1">&#39;Compliments&#39;</span><span class="p">,</span> <span class="s1">&#39;Criticisms&#39;</span><span class="p">]]</span>
</pre>

    



<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt input_prompt">Out&nbsp;[8]:
<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Restaurant</th>
      <th>Review</th>
      <th>Review_tokenized</th>
      <th>Compliments</th>
      <th>Criticisms</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>ที่สำคัญของร้านนี้คือบริการดีมากพนักงานน่ารักส...</td>
      <td>บริการ,ดีมาก,พนักงาน,น่ารัก,สะอาดสะอ้าน,ใส่ใจ,...</td>
      <td>[​ดี, น้ำจิ้มอร่อย, รสชาติดี, โมจิอร่อย, ไอติม...</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>นึกถึงชาบูญี่ปุ่นยังไงก็ต้อง คิดถึงโมโม่ พาราไ...</td>
      <td>นึกถึง,ชาบู,ญี่ปุ่น,คิดถึง,โม,โม่,พาราไดซ์,คุณ...</td>
      <td>[บริการดี, พนักงานบริการดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>มาทานช่วงนี้ สามารถนั่งโต๊ะเดียวกัน หม้อเดียวก...</td>
      <td>ทาน,นั่ง,โต๊ะ,หม้อ,โต๊ะ,ยังมี,ฉาก,กั้น,น้ำ,ซุป...</td>
      <td>[:อร่อย, ตะอร่อย, แดงอร่อย, ::อร่อย]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>ถ้านึกถึงชาบูที่มีเนื้อเน้นๆ ในราคาไม่โหดจนเกิ...</td>
      <td>นึกถึง,ชาบู,เนื้อ,ราคา,โหด,เกินไป,นึกถึง,โม,โม...</td>
      <td>[เกินไปอร่อย]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>เดินมาหน้าร้านแล้วได้กลิ่นชาบูหอมมาก ๆ  ประกอบ...</td>
      <td>เดิน,หน้า,ได้กลิ่น,ชาบู,หอ,มมาก,โปร,บัตรเครดิต...</td>
      <td>[ขนมอร่อย]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>ร้านบุฟเฟ่ ชาบูแนวญี่ปุ่น สายเนื้อหมู เนื้อวัว...</td>
      <td>บุ,ฟเฟ่,ชาบู,แนว,ญี่ปุ่น,สาย,เนื้อหมู,เนื้อวัว...</td>
      <td>[บ๋วยอร่อย, อร่อยดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>Number 20 : โมโม – พาราไดส์ (สาขาเดอะมอลบางกะป...</td>
      <td>Number,20,:,โม,โม,–,พารา,ได,ส์,สาขา,เดอะ,มอ,ล,...</td>
      <td>[]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ</td>
      <td>ร้านชาบูเฟรนไชส์รสชาติดีมากคุ้มค่าเหมาะสมกับรา...</td>
      <td>ชาบู,เฟรนไชส์,รสชาติ,ดีมาก,คุ้มค่า,เหมาะสม,ราค...</td>
      <td>[บ๊วยดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>มา านที่ขาบูชิต้องมาตอนหิว ไม่งั้นจะไม่คุ้มนะค...</td>
      <td>าน,ขา,บู,ชิ,ตอน,หิว,ไม่งั้น,คุ้ม,ฮ่า,เมนู,ของก...</td>
      <td>[]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>ใครชอบกุ้งทอดเทมปุระ แค่กุ้งเทมปุระก็คุ้มแล้ว ...</td>
      <td>ชอบ,กุ้ง,ทอด,เท,ม,ปุระ,กุ้ง,เท,ม,ปุระ,คุ้ม,ทาน...</td>
      <td>[]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>กลับมาอัพเดทราคาชาบูชิ ตอนนี้อยู่ที่ 399 บาท n...</td>
      <td>กลับมา,อัพเดท,ราคา,ชาบู,ชิ,ตอนนี้,399,บาท,net,...</td>
      <td>[เพลินดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>ห่างหายไปนานสำหรับชาบูชิ ตั้งแต่ทางร้านได้ปรับ...</td>
      <td>ห่างหาย,สำหรับ,ชาบู,ชิ,ขึ้นราคา,แถม,ลด,เวลา,คุ...</td>
      <td>[นุ่มดี, หอมอร่อย, ปอกเปลือกดี, สดดี, เด้งดี, ...</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>เมื่อหลายวันก่อนนัดหาข้าวทานกับเดอะแกงค์ และก็...</td>
      <td>วันก่อน,นัด,หา,ข้าว,ทาน,เดอะ,แกงค์,ลงเอย,อาหาร...</td>
      <td>[รสชาติดี, นุ่มอร่อย]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>บอกตรงๆว่าหลายครั้งที่เลือกจะกินบุฟเฟต์จะต้องอ...</td>
      <td>หลายครั้ง,เลือก,กิน,บุฟเฟต์,อาราม,ณ์,รอ,คิว,สุ...</td>
      <td>[เดินดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>สวัสดีครับวันนี้จะขอมารีวิวร้านชาบูชิ บุฟเฟขวั...</td>
      <td>สวัสดี,รีวิว,ชาบู,ชิ,บุ,ฟเฟ,ขวัญใจ,คน,เมนู,เลื...</td>
      <td>[จิ้มอร่อย, ดูอร่อย]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Shabushi (ชาบูชิ) เดอะมอลล์บางกะปิ ชั้น G</td>
      <td>ชาบูชิ สาขาเดอะมอลล์บางกะปิ ตั้งอยู่ประตูหน้าห...</td>
      <td>ชาบู,ชิ,สาขา,เดอะ,มอลล์,กะปิ,ตั้งอยู่,ประตู,หน...</td>
      <td>[ขนาดดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>16</th>
      <td>ข้าน้อยขอชาบู</td>
      <td>หลังจากที่เคยลองสาขายูเนี่ยนมอลล์ไป รอบนี้มาที...</td>
      <td>หลังจากที่,ลอง,สาขา,ยู,นม,อลล์,รอบ,สาขา,เดอะ,ม...</td>
      <td>[คุ้มดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>17</th>
      <td>ข้าน้อยขอชาบู</td>
      <td>ร้านบุฟเฟ่ต์ที่มีโปรโมชั่นหัวละ199บาท ไม่รวมน้...</td>
      <td>บุฟเฟ่ต์,โปรโมชั่น,หัว,199,บาท,น้ำ,VAT,ทาน,ธรร...</td>
      <td>[ต้มยำอร่อย, อร่อยดี, รับประกันอร่อย, โทสฟรี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>18</th>
      <td>ข้าน้อยขอชาบู</td>
      <td>เคยรู้จักร้านนี้จากที่ union mall ไม่เคยได้ลอง...</td>
      <td>รู้จัก,union,mall,ลอง,กิน,จำได้,ขึ้นใจ,ชื่อ,😆,...</td>
      <td>[รู้สึกอร่อย, บริการดี]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ข้าน้อยขอชาบู</td>
      <td>อร่อยมากค่ะ คุ้มค่าสมราคา บุฟเฟ่หมูผักต่างๆ รว...</td>
      <td>อร่อย,คุ้มค่า,สมราคา,บุ,ฟเฟ่,หมู,ผัก,น้ำ,จบ,25...</td>
      <td>[สวยอร่อย, เฟ่ดี, งานดี, ของหวานอร่อย, ครีมอร่อย]</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>20</th>
      <td>ข้าน้อยขอชาบู</td>
      <td>ก็ตามที่เขียนเลยครับ ว่า ถ้าจะจ่ายในราคา 199 บ...</td>
      <td>จ่าย,ราคา,199,บาท,จ่าย,เงินสด,ราคา,น้ำ,VAT,7,%...</td>
      <td>[หน้าอร่อย, สดๆ ร้อนๆอร่อย]</td>
      <td>[]</td>
    </tr>
  </tbody>
</table>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre>

 


</html>

