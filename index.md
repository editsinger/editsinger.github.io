# <center>EditSinger: Zero-Shot Text-Based Singing Voice Editing System with Diverse Prosody Modeling</center>

## Abstract

Zero-shot text-based singing voice editing enables users to edit the singing content by just performing text operations on the lyrics, while  without any additional data from the target singer. However, due to the different demands, challenges occur when applying existing speech editing methods to singing voice editing task, mainly including the lack of systematic consideration concerning prosody in insertion and deletion, as well as the trade-off between the naturalness of pronunciation and the preservation of prosody in replacement. In this paper we propose EditSinger, which is a novel singing voice editing model with specially designed diverse prosody modules to overcome the challenges above. Specifically, 1) a general masked variance adaptor is introduced for the comprehensive prosody modeling of the inserted lyrics and the transition of deletion boundary; and 2) we further design a fusion pitch predictor for replacement. By disentangling the reference pitch and fusing the predicted pronunciation, the edited pitch can be reconstructed, which could ensure a natural pronunciation while preserving the prosody of the original audio. In addition, to the best of our knowledge, it is the first zero-shot text-based singing voice editing system. Our experiments conducted on the OpenSinger prove that EditSinger can synthesize high-quality edited singing voices with natural prosody according to the corresponding operations.

<img align="center" src="resources/pipeline.png" style="  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 80%;" />

<img align="center" src="resources/model.png" style="  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 80%;" />

**Introduction:**<br> 
<li>In the first two sections (<strong>Audio Samples & Method Analyses</strong>), there are some samples of common performance demonstration and comparison experiments.</li>
<li>In the third section (<strong>More Samples</strong>), we provide more samples of different aspects (e.g., comparisons of different editing positions).</li>
<li>Our research is based on the open-source dataset OpenSinger, and all experiments conducted in the paper have been authorized by the publisher. This project is currently only used for research, and aims to make contributions and provides some ideas for the community. Please do not used for commercial purposes.</li>
## 1 Audio Samples
*Notes:* <br>
<li>GT denotes the original audio(the input audio to be edited).</li>
<li>The <font color="red">red</font> part represents the editing region.</li>
<li>Words ?????? Phonemes</li>
#### Exp. 1:

original lyrics: ???????????????????????? ?????? <BOS> p eng | y ou # ai | d e # n a | m e # k u | t ong <EOS> <br>
insertion: ??????<font color="red">??????</font>?????????????????? ?????? <BOS> p eng | y ou # <font color="red">r u | g uo #</font> ai # d e # n a | m e # k u | t ong <EOS> <br>
replacement: ??????????????????<font color="red">??????(<strike>??????</strike>)</font> ?????? <BOS> p eng | y ou # ai # d e # n a | m e # <font color="red">r en | zh en (<strike> k u | t ong</strike>) </font> <EOS> <br>
deletion: ????????????<font color="red">(<strike>??????</strike>)</font>?????? ?????? <BOS> p eng | y ou # ai # d e # <font color="red"> (<strike> n a | m e #</strike>) </font>k u | t ong <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>EditSinger(replacement)</th>
            <th>EditSinger(deletion)</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000001.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000001.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000001.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(replacement)/0000000001.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000001.wav" type="audio/wav"></audio></td>
        </tr>
    </tbody>
    </table>
</div>


#### Exp. 2:
original lyrics: ????????????????????? ?????? <BOS> ai # k e | y i # b u | w en # d ui | c uo <EOS> <br>
insertion: ???<font color="red">??????</font>?????????????????? ?????? <BOS> ai # <font color="red">z en | m e #</font> k e | y i # b u | w en # d ui | c uo <EOS> <br>
replacement: ???<font color="red">??????(<strike>??????</strike>)</font>???????????? ?????? <BOS> ai #<font color="red"> z en | m e # (<strike>k e | y i #</strike>) </font>  b u | w en # d ui | c uo <EOS> <br>
deletion: ???<font color="red">(<strike>??????</strike>)</font>???????????? ?????? <BOS> ai # <font color="red">(<strike> k e | y i #</strike>)</font> b u | w en # d ui | c uo <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>EditSinger(replacement)</th>
            <th>EditSinger(deletion)</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000002.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000002.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000002.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(replacement)/0000000002.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000002.wav" type="audio/wav"></audio></td>
        </tr>
    </tbody>
    </table>
</div>

#### Exp. 3:
original lyrics: ?????????????????????????????? ?????? <BOS> n i # h e | k u # f ei | w ei # t a # d eng # z ai # y u # zh ong <EOS> <br>
insertion: ??????????????????<font color="red">??????</font>???????????? ?????? <BOS> n i # h e | k u # f ei | w ei # t a # <font color="red">sh a | sh a #</font> d eng # z ai # y u # zh ong <EOS> <br>
replacement: ??????????????????<font color="red">?????????(<strike>?????????</strike>)</font>??? ?????? <BOS> n i # h e | k u # f ei | w ei # t a # <font color="red">zh u | l i # f eng | (<strike> d eng # z ai # y u #</strike>)</font> zh ong <EOS> <br>
deletion: ???<font color="red">(<strike>?????????</strike>)</font>?????????????????? ?????? <BOS> n i # <font color="red">(<strike> h e | k u # f ei |</strike>)</font> w ei # t a # d eng # z ai # y u # zh ong <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>EditSinger(replacement)</th>
            <th>EditSinger(deletion)</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000003.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000003.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000003.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(replacement)/0000000003.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000003.wav" type="audio/wav"></audio></td>
        </tr>
    </tbody>
    </table>
</div>

#### Exp. 4:
original lyrics: ??????????????????????????????????????? ?????? <BOS> j i | d uo # y un # z ai # y in | t ian # w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
insertion: ??????<font color="red">?????????</font>????????????????????????????????? ?????? <BOS> j i | d uo # <font color="red">g u | d u # d e #</font> y un # z ai # y in | t ian # w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
replacement: ???<font color="red">??????(<strike>??????</strike>)</font>?????????????????????????????? ?????? <BOS> j i | <font color="red">p ian # y e | (<strike>d uo # y un #</strike>)</font> z ai # y in | t ian # w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
deletion: ?????????<font color="red">(<strike>?????????</strike>)</font>????????????????????? ?????? <BOS> j i | d uo # y un | <font color="red">(<strike>z ai # y in | t ian #</strike>)</font> w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>EditSinger(replacement)</th>
            <th>EditSinger(deletion)</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000011.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(replacement)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000011.wav" type="audio/wav"></audio></td>
        </tr>
    </tbody>
    </table>
</div>

#### Exp. 5:
original lyrics: ?????????????????? ?????? <BOS> b ei # ch ui | j in # l e # z uo | er <EOS> <br>
insertion: ???<font color="red">??????</font>??????????????? ?????? <BOS> b ei # <font color="red">s i | n ian #</font> ch ui | j in # l e # z uo | er <EOS> <br>
replacement: ???<font color="red">?????????(<strike>?????????</strike>)</font>?????? ?????? <BOS> b ei # <font color="red">ch uan | d i # d ao # (<strike>ch ui | j in # l e # </strike>)</font> z uo | er <EOS> <br>
deletion: ?????????<font color="red">(<strike>???</strike>)</font>?????? ?????? <BOS> b ei # ch ui | j in # <font color="red">(<strike>l e #</strike>)</font> z uo | er <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>EditSinger(replacement)</th>
            <th>EditSinger(deletion)</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000012.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000012.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000012.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(replacement)/0000000012.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000012.wav" type="audio/wav"></audio></td>
        </tr>
    </tbody>
    </table>
</div>

#### Exp. 6:
original lyrics: ?????????????????? ?????? <BOS> z ai # h un | an # zh ong # d e # w o <EOS> <br>
insertion: ???<font color="red">??????</font>??????????????? ?????? <BOS> z ai # <font color="red">n a | sh i #</font> h un | an # zh ong # d e # w o <EOS> <br>
replacement: ????????????<font color="red">??????(<strike>??????</strike>)</font> ?????? <BOS> z ai # h un | an # zh ong # <font color="red">y u # n i (<strike>d e # w o</strike>)</font> <EOS> <br>
deletion: ?????????<font color="red">(<strike>???</strike>)</font>?????? ?????? <BOS> z ai # h un | an # <font color="red">(<strike> zh ong # </strike>)</font> d e # w o <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>EditSinger(replacement)</th>
            <th>EditSinger(deletion)</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000013.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000013.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000013.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(replacement)/0000000013.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000013.wav" type="audio/wav"></audio></td>
        </tr>
    </tbody>
    </table>
</div>

## 2 Method Analyses
### 2.1 Insertion
#### Exp. 1:
original lyrics: ???????????????????????? ?????? <BOS> p eng | y ou # ai | d e # n a | m e # k u | t ong <EOS> <br>
insertion: ??????<font color="red">??????</font>?????????????????? ?????? <BOS> p eng | y ou # <font color="red">r u | g uo #</font> ai # d e # n a | m e # k u | t ong <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>w/o CVA</th>
            <th>w/o ML-GAN</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000001.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000001.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000001.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/insertion/womva/0000000001.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/insertion/wogan/0000000001.wav" type="audio/wav"></audio></td>
        </tr>
            
    </tbody>
    </table>
</div>

#### Exp. 2:
original lyrics: ??????????????????????????????????????? ?????? <BOS> j i | d uo # y un # z ai # y in | t ian # w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
insertion: ??????<font color="red">?????????</font>????????????????????????????????? ?????? <BOS> j i | d uo # <font color="red">g u | d u # d e #</font> y un # z ai # y in | t ian # w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(insertion)</th>
            <th>w/o CVA</th>
            <th>w/o ML-GAN</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000011.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(insertion)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/insertion/womva/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/insertion/wogan/0000000011.wav" type="audio/wav"></audio></td>
        </tr>
            

    </tbody>
    </table>
</div>

### 2.2 Deletion

#### Exp. 1:
original lyrics: ?????????????????????????????? ?????? <BOS> n i # h e | k u # f ei | w ei # t a # d eng # z ai # y u # zh ong <EOS> <br>
deletion: ???<font color="red">(<strike>?????????</strike>)</font>?????????????????? ?????? <BOS> n i # <font color="red">(<strike>h e | k u # f ei | </strike>)</font> w ei # t a # d eng # z ai # y u # zh ong <EOS>> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(deletion)</th>
            <th>w/o CVA</th>
            <th>w/o ML-GAN</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000003.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000003.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000003.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/deletion/womva/0000000003.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/deletion/wogan/0000000003.wav" type="audio/wav"></audio></td>
        </tr>
        

        
    </tbody>
    </table>
</div>

#### Exp. 2:
original lyrics: ??????????????????????????????????????? ?????? <BOS> j i | d uo # y un # z ai # y in | t ian # w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
deletion: ?????????<font color="red">(<strike>?????????</strike>)</font>????????????????????? ?????? <BOS> j i | d uo # y un | <font color="red">(<strike>z ai # y in | t ian #</strike>)</font> w ang # l e # g ai # w ang # n a | r # z ou <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(deletion)</th>
            <th>w/o CVA</th>
            <th>w/o ML-GAN</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000011.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/editsinger(deletion)/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/deletion/womva/0000000011.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/CMOS/deletion/wogan/0000000011.wav" type="audio/wav"></audio></td>
        </tr>
            
            
    </tbody>
    </table>
</div>

### 2.3 Replacement (Dang Test)
*Note: In this part, it can fully demonstrate the superior performance of Editsinger(replacement), and it even supports the replacement of entire sentences, which is not available in previous work. "Dang" here can be understood as any character. We have tested many other characters and done experiments including part and whole sentence replacement experiments, which are also very effective. Directly migrating the original prosody (Direct) to the new word without considering the attributes of the word will lead to a decrease in the sense of hearing, and ignoring the prosody of the corresponding position (w/o FPIP) will lead to a decrease in the similarity with the original song.*
#### Exp. 1:
original lyrics: ??????????????????????????? ?????? <BOS> x iang # d ang # d ang # n i # x in # k ou | l i # d e # f eng <EOS> <br>
replacement: ??????????????????????????? ?????? <BOS> d ang | d ang # d ang | d ang # d ang | d ang # d ang | d ang # d ang <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(replacement)</th>
            <th>Direct</th>
            <th>w/o FPIP</th>
            <th>w/o VQVAE</th>
            <th>w/o ML-GAN</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000004.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000004.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/editsinger(replacement)/0000000004.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/direct/0000000004.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/context-aware/0000000004.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/wovqvae/0000000004.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/wogan/0000000004.wav" type="audio/wav"></audio></td>
        </tr>

    </tbody>
    </table>
</div>

#### Exp. 2:
original lyrics: ?????????????????? ?????? <BOS> t ing # y in | t ian # sh uo # sh en | m e <EOS> <br>
replacement: ?????????????????? ?????? <BOS> d ang | d ang # d ang | d ang # d ang | d ang <EOS> <br>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>GT</th>
            <th>GT(Mel+PWG)</th>
            <th>EditSinger(replacement)</th>
            <th>Direct</th>
            <th>w/o FPIP</th>
            <th>w/o VQVAE</th>
            <th>w/o ML-GAN</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT/0000000020.mp3" type="audio/mp3"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000020.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/editsinger(replacement)/0000000020.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/direct/0000000020.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/context-aware/0000000020.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/wovqvae/0000000020.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/MOS2/wogan/0000000020.wav" type="audio/wav"></audio></td>
        </tr>
    
        
    </tbody>
    </table>
</div>



## 3 More Samples
### Editing at Different Positions (Begining/Middle/End of the Sentence)
original lyrics: ???????????????????????? ?????? <BOS> p eng | y ou # ai | d e # n a | m e # k u | t ong <EOS> <br>
<audio style="width: 150px;" controls="" ><source src="resources/MOS1/GT(mel+pwg)/0000000001.wav" type="audio/wav"></audio>
<div>
    <table style='width: 100%;'>
        <thead>
        <tr>
            <th>Type</th>
            <th>Begining</th>
            <th>Middle</th>
            <th>End</th>
        </tr>
        </thead>
        <tbody>

        <tr>
	  <td>insertion</td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/more/insertion/0000000001_1.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/more/insertion/0000000001_2.wav" type="audio/wav"></audio></td>
  	  <td><audio style="width: 150px;" controls="" ><source src="resources/more/insertion/0000000001_3.wav" type="audio/wav"></audio></td>
        </tr>    

        <tr>
	  <td></td>
            <td><font color="red">??????</font>????????????????????????</td>
            <td>??????<font color="red">??????</font>??????????????????</td>
	  <td>????????????????????????<font color="red">??????</font></td>
        </tr>  

        <tr>
	  <td>deletion</td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/more/deletion/0000000001_1.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/more/deletion/0000000001_2.wav" type="audio/wav"></audio></td>
  	  <td><audio style="width: 150px;" controls="" ><source src="resources/more/deletion/0000000001_3.wav" type="audio/wav"></audio></td>
        </tr>    
       

        <tr>
	  <td></td>
            <td><font color="red">(<strike>??????</strike>)</font>??????????????????</td>
 	  <td>????????????<font color="red">(<strike>??????</strike>)</font>??????</td>
	  <td>?????????????????????<font color="red">(<strike>???</strike>)</font></td>
        </tr>    

	<tr>
	  <td>replacement</td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/more/replacement/0000000001_1.wav" type="audio/wav"></audio></td>
            <td><audio style="width: 150px;" controls="" ><source src="resources/more/replacement/0000000001_2.wav" type="audio/wav"></audio></td>
  	  <td><audio style="width: 150px;" controls="" ><source src="resources/more/replacement/0000000001_3.wav" type="audio/wav"></audio></td>
        </tr>    

	<tr>
	  <td></td>
            <td><font color="red">??????(<strike>??????</strike>)</font>??????????????????</td>
            <td>????????????<font color="red">??????(<strike>??????</strike>)</font>??????</td>
	  <td>??????????????????<font color="red">??????(<strike>??????</strike>)</font></td>
        </tr>    

    </tbody>
    </table>
</div>
