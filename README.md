# BenchmarkJieBa.NET


```sh

sudo docker run --name webapi_benchmark -d -p 9090:9090 ikende/beetlex_webapi_benchmark:v0.8.6

```


# jieba.NET ��[jieba���ķִ�](https://github.com/fxsjy/jieba)��.NET�汾��C#ʵ�֣���

��ǰ�汾Ϊ0.42.2������jieba 0.42���ṩ��jieba**����һ��**�Ĺ�����ӿڣ�����֧�������µ�paddleģʽ������jieba��ʵ��˼·�����Կ���[��ƪwiki](https://github.com/anderscui/jieba.NET/wiki/%E7%90%86%E8%A7%A3%E7%BB%93%E5%B7%B4%E5%88%86%E8%AF%8D)���ᵽ�����ϡ�

���⣬Ҳ�ṩ�� `KeywordProcessor`���ο� [FlashText](https://github.com/vi3k6i5/flashtext) ʵ�֡�`KeywordProcessor` ���Ը����ش��ı�����ȡ**�ʵ��еĹؼ���**��������Դ�Сд�����ո�Ĵʵȡ�

������ڿ�����������ִ��йص���������ѣ����ύһ��Issue��I see u:)

## �ص�

* ֧�����ִַ�ģʽ��
    - ��ȷģʽ����ͼ�������ȷ���п����ʺ�**�ı�����**��
    - ȫģʽ���Ѿ��������еĿ��ԳɴʵĴ��ﶼɨ�����, **�ٶȷǳ��죬���ǲ��ܽ������**��������˵���ִʹ��̲�������ڴ�Ƶ����������·�����಻��ʹ��HMM��
    - ��������ģʽ���ھ�ȷģʽ�Ļ����ϣ��Գ����ٴ��з֣�����ٻ��ʣ�**�ʺ�������������ִ�**��
* ֧��**����ִ�**
* ֧������Զ���ʵ���Զ����
* MIT ��ȨЭ��

## �㷨

* ����ǰ׺�ʵ�ʵ�ָ�Ч�Ĵ�ͼɨ�裬���ɾ����к������п��ܳɴ���������ɵ������޻�ͼ (DAG)
* �����˶�̬�滮����������·��, �ҳ����ڴ�Ƶ������з����
* ����δ��¼�ʣ������˻��ں��ֳɴ�������HMMģ�ͣ�ʹ����Viterbi�㷨

## ��װ������

��ǰ�汾֧��net40��net45��netstandard2.0�������ֶ�������Ŀ��Ҳ����ͨ��NuGet������ã�

```shell
PM> Install-Package jieba.NET
```

��װ֮����packages\jieba.NETĿ¼�¿��Կ���ResourcesĿ¼����������jieba.NET��������Ĵʵ估���������ļ�����򵥵����÷����ǽ�����ResourcesĿ¼��������������Ŀ¼������jieba.NET��ʹ�����õ�Ĭ������ֵ�����ϣ������Щ�ļ���������λ�ã���Ҫ��app.config��web.config��������µ������

```xml
<appSettings>
    <add key="JiebaConfigFileDir" value="C:\jiebanet\config" />
</appSettings>
```

��Ҫע����ǣ����·������ʹ�þ���·�������·����**���ʹ�����·������ôjieba.NET������·��������ڵ�ǰӦ�ó������BaseDirectory**��

����ʾ����

* ���þ���·��ʱ������������ΪC:\jiebanet\config����ô���ʵ��·����ƴ��Ϊ��C:\jiebanet\config\dict.txt��
* �������·��ʱ����δ����κ��������ô����ʹ��Ĭ�ϵ�**���·����Resources**��������������Ϊ..\config����ͨ��..���������·����������ǰӦ�ó������BaseDirectory��C:\myapp\bin\����ô���ʵ��·����ƴ��Ϊ��C:\myapp\config\dict.txt��

### ʹ�ô������ôʵ�·��

�����ΪĳЩԭ�򣬲�����ͨ��Ӧ�õ� config �ļ����ã���ʹ�ô������ã���ʹ���κηִʹ���֮ǰ������ʹ�þ���·�������磺

```c#
JiebaNet.Segmenter.ConfigManager.ConfigFileBaseDir = @"C:\jiebanet\config";
```

## ��Ҫ����

### 1. �ִ�

* `JiebaSegmenter.Cut`���������������������textΪ���ִʵ��ַ�����cutAllָ���Ƿ����ȫģʽ��hmmָ��ʹ���Ƿ�ʹ��hmmģ���з�δ��¼�ʣ���������Ϊ`IEnumerable<string>`
* `JiebaSegmenter.CutForSearch`���������������������textΪ���ִʵ��ַ�����hmmָ��ʹ���Ƿ�ʹ��hmmģ�ͣ���������Ϊ`IEnumerable<string>`

����ʾ��

```c#
var segmenter = new JiebaSegmenter();
var segments = segmenter.Cut("�����������廪��ѧ", cutAll: true);
Console.WriteLine("��ȫģʽ����{0}", string.Join("/ ", segments));

segments = segmenter.Cut("�����������廪��ѧ");  // Ĭ��Ϊ��ȷģʽ
Console.WriteLine("����ȷģʽ����{0}", string.Join("/ ", segments));

segments = segmenter.Cut("�����������׺��д���");  // Ĭ��Ϊ��ȷģʽ��ͬʱҲʹ��HMMģ��
Console.WriteLine("���´�ʶ�𡿣�{0}", string.Join("/ ", segments));

segments = segmenter.CutForSearch("С��˶ʿ��ҵ���й���ѧԺ�������������ձ�������ѧ����"); // ��������ģʽ
Console.WriteLine("����������ģʽ����{0}", string.Join("/ ", segments));

segments = segmenter.Cut("�����ĺ���δ������");
Console.WriteLine("��������������{0}", string.Join("/ ", segments));
```

���

```
��ȫģʽ������/ ����/ ����/ �廪/ �廪��ѧ/ ����/ ��ѧ
����ȷģʽ������/ ����/ ����/ �廪��ѧ
���´�ʶ�𡿣���/ ����/ ��/ ����/ ����/ ����
����������ģʽ����С��/ ˶ʿ/ ��ҵ/ ��/ �й�/ ��ѧ/ ѧԺ/ ��ѧԺ/ �й���ѧԺ/ ����/ ������/ ��/ ��/ ��/ �ձ�/ ����/ ��ѧ/ �ձ�������ѧ/ ����
�������������������/ ��/ ��/ ��δ/ �����/ ��
```


### 2. ����Զ���ʵ�

#### ���شʵ�

* �����߿���ָ���Զ���Ĵʵ䣬�Ա����jieba�ʿ���û�еĴʡ���Ȼjieba���´�ʶ��������������������´ʿ��Ա�֤���ߵ���ȷ��
* `JiebaSegmenter.LoadUserDict("user_dict_file_path")`
* �ʵ��ʽ�����ʵ��ʽ��ͬ����һ�а������ʡ���Ƶ����ʡ�ԣ������ԣ���ʡ�ԣ����ÿո����
* ��Ƶʡ��ʱ���ִ�����ʹ���Զ�������Ĵ�Ƶ��֤�ôʱ��ֳ�

��

```
���°� 3 i
�Ƽ��� 5
�P���� nz
̨��
����ѧϰ 3
```

#### �����ʵ�

* ʹ��`JiebaSegmenter.AddWord(word, freq=0, tag=null)`�����һ���´ʣ��������֪�ʵĴ�Ƶ����`freq`��������������ʹ���Զ�������Ĵ�Ƶ��������Ĵ�Ƶ�ɱ�֤�ôʱ��ֳ���
* ʹ��`JiebaSegmenter.DeleteWord(word)`���Ƴ�һ���ʣ�ʹ�䲻�ܱ��ֳ���

### 3. �ؼ�����ȡ

#### ����TF-IDF�㷨�Ĺؼ�����ȡ

* `JiebaNet.Analyser.TfidfExtractor.ExtractTags(string text, int count = 20, IEnumerable<string> allowPos = null)`�ɴ�ָ���ı��г�ȡ���ؼ��ʡ�
* `JiebaNet.Analyser.TfidfExtractor.ExtractTagsWithWeight(string text, int count = 20, IEnumerable<string> allowPos = null)`�ɴ�ָ���ı���**��ȡ�ؼ��ʵ�ͬʱ�õ���Ȩ��**��
* �ؼ��ʳ�ȡ���������ļ�Ƶ�ʣ�IDF�����������һ��IDF���Ͽ⣬��������Ϊ�����Զ�������Ͽ⡣
* �ؼ��ʳ�ȡ�����ͣ�ôʣ�Stop Words�����������һ��ͣ�ô����Ͽ⣬������Ͽ�ϲ���NLTK��Ӣ��ͣ�ôʺ͹����������ͣ�ôʡ�

#### ����TextRank�㷨�Ĺؼ��ʳ�ȡ

* `JiebaNet.Analyser.TextRankExtractor`��`TfidfExtractor`��ͬ�Ľӿڡ���Ҫע����ǣ�`TextRankExtractor`Ĭ�������ֻ��ȡ���ʺͶ��ʡ�
* �Թ̶����ڴ�С��Ĭ��Ϊ5��ͨ��Span���Ե������ʹ�֮��Ĺ��ֹ�ϵ����ͼ

### 4. ���Ա�ע

* `JiebaNet.Segmenter.PosSeg.PosSegmenter`������ڷִʵ�ͬʱ��Ϊÿ������Ӵ��Ա�ע��
* ���Ա�ע���ú�ictclas���ݵı�Ƿ�������ictclas��jieba��ʹ�õı�Ƿ��б���ο���[���Ա��](https://gist.github.com/luw2007/6016931)��

```c#
var posSeg = new PosSegmenter();
var s = "һ��˶������ĸ��������ƣ���ңԶ�����ص�̫����Ѹ����Ʈ��";

var tokens = posSeg.Cut(s);
Console.WriteLine(string.Join(" ", tokens.Select(token => string.Format("{0}/{1}", token.Word, token.Flag))));
```

```
һ��/m ˶������/i ��/uj ����/n ����/n ��/ns ��/x ��/p ңԶ/a ��/c ����/a ��/uj ̫��/n ��/f Ѹ��/z ��/uv Ʈ��/v
```

### 5. Tokenize�����ش�����ԭ�ĵ���ֹλ��

* Ĭ��ģʽ

```c#
var segmenter = new JiebaSegmenter();
var s = "���ͷ�װ��Ʒ���޹�˾";
var tokens = segmenter.Tokenize(s);
foreach (var token in tokens)
{
    Console.WriteLine("word {0,-12} start: {1,-3} end: {2,-3}", token.Word, token.StartIndex, token.EndIndex);
}
```

```
word ����           start: 0   end: 2
word ��װ           start: 2   end: 4
word ��Ʒ           start: 4   end: 6
word ���޹�˾         start: 6   end: 10
```

* ����ģʽ

```c#
var segmenter = new JiebaSegmenter();
var s = "���ͷ�װ��Ʒ���޹�˾";
var tokens = segmenter.Tokenize(s, TokenizerMode.Search);
foreach (var token in tokens)
{
    Console.WriteLine("word {0,-12} start: {1,-3} end: {2,-3}", token.Word, token.StartIndex, token.EndIndex);
}
```

```
word ����           start: 0   end: 2
word ��װ           start: 2   end: 4
word ��Ʒ           start: 4   end: 6
word ����           start: 6   end: 8
word ��˾           start: 8   end: 10
word ���޹�˾         start: 6   end: 10
```

### 6. ���зִ�

ʹ�����·�����

* `JiebaSegmenter.CutInParallel()`��`JiebaSegmenter.CutForSearchInParallel()`
* `PosSegmenter.CutInParallel()`

### 7. ��Lucene.NET�ļ���

jiebaForLuceneNet��Ŀ�ṩ����Lucene.NET�ļ򵥼��ɣ�������Ϣ�뿴��[jiebaForLuceneNet](https://github.com/anderscui/jiebaForLuceneNet/wiki/%E4%B8%8ELucene.NET%E7%9A%84%E9%9B%86%E6%88%90)

### 8. �����ʵ�

jieba�ִ����ṩ�������Ĵʵ��ļ���

* ռ���ڴ��С�Ĵʵ��ļ� [https://raw.githubusercontent.com/anderscui/jieba.NET/master/ExtraDicts/dict.txt.small](https://raw.githubusercontent.com/anderscui/jieba.NET/master/ExtraDicts/dict.txt.small)
* ֧�ַ���ִʸ��õĴʵ��ļ� [https://raw.githubusercontent.com/anderscui/jieba.NET/master/ExtraDicts/dict.txt.big](https://raw.githubusercontent.com/anderscui/jieba.NET/master/ExtraDicts/dict.txt.big)

### 9. �ִ��ٶ�

* ȫģʽ��2.5 MB/s
* ��ȷģʽ��1.1 MB/s
* ���Ի����� Intel(R) Core(TM) i3-2120 CPU @ 3.30GHz��Χ��.txt��734KB��

### 10. �����зִ�

Segmenter.Cli��Ŀbuild֮��õ�jiebanet.ext������ѡ���ʵ���÷����£�

```shell
-f       --file          the file name, (��Ҫ��).
-d       --delimiter     the delimiter between tokens, default: / .
-a       --cut-all       use cut_all mode.
-n       --no-hmm        don't use HMM.
-p       --pos           enable POS tagging.
-v       --version       show version info.
-h       --help          show help details.

sample usages:
$ jiebanet -f input.txt > output.txt
$ jiebanet -d | -f input.txt > output.txt
$ jiebanet -p -f input.txt > output.txt
```

### 11. ��Ƶͳ��

����ʹ��`Counter`��ͳ�ƴ�Ƶ����ʵ������Python��׼���Counter�ࣨ����ӿں�ʵ��ϸ�����в�ͬ�����÷������ǣ�

```c#
var s = "����ѧ�ͼ������ѧ֮�У��㷨��algorithm��Ϊ�κ�������ľ�����㲽���һ�����У������ڼ��㡢���ݴ�����Զ�������ȷ���ԣ��㷨��һ����ʾΪ���޳��б����Ч�������㷨Ӧ�������������ָ�����ڼ��㺯����";
var seg = new JiebaSegmenter();
var freqs = new Counter<string>(seg.Cut(s));
foreach (var pair in freqs.MostCommon(5))
{
    Console.WriteLine($"{pair.Key}: {pair.Value}");
}
```

�����

```bash
��: 4
��: 3
�㷨: 3
����: 3
��: 3
```

`Counter`���ͨ��`Add`��`Subtract`��`Union`���������޸ģ������`MostCommon`�������Ƶ����ߵ����ɴʡ������÷��ɼ�����������

### 12. KeywordProcessor

��ͨ�� `KeywordProcessor` ��ȡ�ı��еĹؼ��ʣ�����������ȡ�� `KeywordExtractor`��ͬ��`KeywordProcessor` �����Ϊ���ڴʵ���ı����ҳ���֪�Ĵʣ�������ˡ�

jieba�ִʵ�ǰ��ʵ������ܴ�����Դ�Сд�����ո�Ĵ�֮������������**�ı���ȡ**Ӧ���У����Ǻܳ����ĳ�������� `KeywordProcessor` ��Ҫ����Ϊ��ȡ֮�ã����Ƿִʣ�����ͨ�����еķ���������ʵ����һ�ֻ����ֵ�ķִ�ģʽ��

����ʾ����

```c#
var kp = new KeywordProcessor();
kp.AddKeywords(new []{".NET Core", "Java", "C����", "�ֵ� tree", "CET-4", "���� ���"});

var keywords = kp.ExtractKeywords("����Ҫͨ��cet-4���ԣ�ѧϰc���ԡ�.NET core������ ��̡�JavaScript�������ֵ� tree���÷�");

// keywords ֵΪ��
// new List<string> { "CET-4", "C����", ".NET Core", "���� ���", "�ֵ� tree"}

// ���Կ���������еĴ��뿪ʼ��ӵĹؼ�����ͬ������������еĴ��򲻾���ͬ�������Ҫ���ؾ����ҵ���ԭ�ʣ�����ʹ�� `raw` ������

var keywords = kp.ExtractKeywords("����Ҫͨ��cet-4���ԣ�ѧϰc���ԡ�.NET core������ ��̡�JavaScript�������ֵ� tree���÷�", raw: true);

// keywords ֵΪ��
// new List<string> { "cet-4", "c����", ".NET core", "���� ���", "�ֵ� tree"}
```




# jieba @ Python
========
����͡����ķִʣ�����õ� Python ���ķִ����

"Jieba" (Chinese for "to stutter") Chinese text segmentation: built to be the best Python Chinese word segmentation module.

- _Scroll down for English documentation._


�ص�
========
* ֧�����ִַ�ģʽ��
    * ��ȷģʽ����ͼ�������ȷ���п����ʺ��ı�������
    * ȫģʽ���Ѿ��������еĿ��ԳɴʵĴ��ﶼɨ�����, �ٶȷǳ��죬���ǲ��ܽ�����壻
    * ��������ģʽ���ھ�ȷģʽ�Ļ����ϣ��Գ����ٴ��з֣�����ٻ��ʣ��ʺ�������������ִʡ�
    * paddleģʽ������PaddlePaddle���ѧϰ��ܣ�ѵ�����б�ע��˫��GRU������ģ��ʵ�ִַʡ�ͬʱ֧�ִ��Ա�ע��paddleģʽʹ���谲װpaddlepaddle-tiny��`pip install paddlepaddle-tiny==1.6.1`��Ŀǰpaddleģʽ֧��jieba v0.40�����ϰ汾��jieba v0.40���°汾��������jieba��`pip install jieba --upgrade` ��[PaddlePaddle����](https://www.paddlepaddle.org.cn/)
* ֧�ַ���ִ�
* ֧���Զ���ʵ�
* MIT ��ȨЭ��

��װ˵��
=======

����� Python 2/3 ������

* ȫ�Զ���װ��`easy_install jieba` ���� `pip install jieba` / `pip3 install jieba`
* ���Զ���װ�������� http://pypi.python.org/pypi/jieba/ ����ѹ������ `python setup.py install`
* �ֶ���װ���� jieba Ŀ¼�����ڵ�ǰĿ¼���� site-packages Ŀ¼
* ͨ�� `import jieba` ������
* �����Ҫʹ��paddleģʽ�µķִʺʹ��Ա�ע���ܣ����Ȱ�װpaddlepaddle-tiny��`pip install paddlepaddle-tiny==1.6.1`��

�㷨
========
* ����ǰ׺�ʵ�ʵ�ָ�Ч�Ĵ�ͼɨ�裬���ɾ����к������п��ܳɴ���������ɵ������޻�ͼ (DAG)
* �����˶�̬�滮����������·��, �ҳ����ڴ�Ƶ������з����
* ����δ��¼�ʣ������˻��ں��ֳɴ������� HMM ģ�ͣ�ʹ���� Viterbi �㷨

��Ҫ����
=======
1. �ִ�
--------
* `jieba.cut` ���������ĸ��������: ��Ҫ�ִʵ��ַ�����cut_all �������������Ƿ����ȫģʽ��HMM �������������Ƿ�ʹ�� HMM ģ�ͣ�use_paddle �������������Ƿ�ʹ��paddleģʽ�µķִ�ģʽ��paddleģʽ�����ӳټ��ط�ʽ��ͨ��enable_paddle�ӿڰ�װpaddlepaddle-tiny������import��ش��룻
* `jieba.cut_for_search` ��������������������Ҫ�ִʵ��ַ������Ƿ�ʹ�� HMM ģ�͡��÷����ʺ������������湹�����������ķִʣ����ȱȽ�ϸ
* ���ִʵ��ַ��������� unicode �� UTF-8 �ַ�����GBK �ַ�����ע�⣺������ֱ������ GBK �ַ����������޷�Ԥ�ϵش������� UTF-8
* `jieba.cut` �Լ� `jieba.cut_for_search` ���صĽṹ����һ���ɵ����� generator������ʹ�� for ѭ������÷ִʺ�õ���ÿһ������(unicode)��������
* `jieba.lcut` �Լ� `jieba.lcut_for_search` ֱ�ӷ��� list
* `jieba.Tokenizer(dictionary=DEFAULT_DICT)` �½��Զ���ִ�����������ͬʱʹ�ò�ͬ�ʵ䡣`jieba.dt` ΪĬ�Ϸִ���������ȫ�ִַ���غ������Ǹ÷ִ�����ӳ�䡣

����ʾ��

```python
# encoding=utf-8
import jieba

jieba.enable_paddle()# ����paddleģʽ�� 0.40��֮��ʼ֧�֣����ڰ汾��֧��
strs=["�����������廪��ѧ","ƹ������������","�й���ѧ������ѧ"]
for str in strs:
    seg_list = jieba.cut(str,use_paddle=True) # ʹ��paddleģʽ
    print("Paddle Mode: " + '/'.join(list(seg_list)))

seg_list = jieba.cut("�����������廪��ѧ", cut_all=True)
print("Full Mode: " + "/ ".join(seg_list))  # ȫģʽ

seg_list = jieba.cut("�����������廪��ѧ", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # ��ȷģʽ

seg_list = jieba.cut("�����������׺��д���")  # Ĭ���Ǿ�ȷģʽ
print(", ".join(seg_list))

seg_list = jieba.cut_for_search("С��˶ʿ��ҵ���й���ѧԺ�������������ձ�������ѧ����")  # ��������ģʽ
print(", ".join(seg_list))
```

���:

    ��ȫģʽ��: ��/ ����/ ����/ �廪/ �廪��ѧ/ ����/ ��ѧ

    ����ȷģʽ��: ��/ ����/ ����/ �廪��ѧ

    ���´�ʶ�𡿣���, ����, ��, ����, ����, ����    (�˴��������С���û���ڴʵ��У�����Ҳ��Viterbi�㷨ʶ�������)

    ����������ģʽ���� С��, ˶ʿ, ��ҵ, ��, �й�, ��ѧ, ѧԺ, ��ѧԺ, �й���ѧԺ, ����, ������, ��, ��, �ձ�, ����, ��ѧ, �ձ�������ѧ, ����

2. ����Զ���ʵ�
----------------

### ����ʵ�

* �����߿���ָ���Լ��Զ���Ĵʵ䣬�Ա���� jieba �ʿ���û�еĴʡ���Ȼ jieba ���´�ʶ��������������������´ʿ��Ա�֤���ߵ���ȷ��
* �÷��� jieba.load_userdict(file_name) # file_name Ϊ�ļ��������Զ���ʵ��·��
* �ʵ��ʽ�� `dict.txt` һ����һ����ռһ�У�ÿһ�з������֣������Ƶ����ʡ�ԣ������ԣ���ʡ�ԣ����ÿո������˳�򲻿ɵߵ���`file_name` ��Ϊ·��������Ʒ�ʽ�򿪵��ļ������ļ�����Ϊ UTF-8 ���롣
* ��Ƶʡ��ʱʹ���Զ�������ܱ�֤�ֳ��ôʵĴ�Ƶ��

**���磺**

```
���°� 3 i
�Ƽ��� 5
�P���� nz
̨��
```

* ���ķִ�����Ĭ��Ϊ `jieba.dt`���� `tmp_dir` �� `cache_file` ���ԣ��ɷֱ�ָ�������ļ����ڵ��ļ��м����ļ������������޵��ļ�ϵͳ��

* ������

    * �Զ���ʵ䣺https://github.com/fxsjy/jieba/blob/master/test/userdict.txt

    * �÷�ʾ����https://github.com/fxsjy/jieba/blob/master/test/test_userdict.py


        * ֮ǰ�� ��С�� / �� / ���� / �� / ���� / Ҳ / �� / �� / ���� / ���� / �� / ר�� /

        * �����Զ���ʿ�󣺡���С�� / �� / ���°� / ���� / Ҳ / �� / �Ƽ��� / ���� / �� / ר�� /

### �����ʵ�

* ʹ�� `add_word(word, freq=None, tag=None)` �� `del_word(word)` ���ڳ����ж�̬�޸Ĵʵ䡣
* ʹ�� `suggest_freq(segment, tune=True)` �ɵ��ڵ�������Ĵ�Ƶ��ʹ���ܣ����ܣ����ֳ�����

* ע�⣺�Զ�����Ĵ�Ƶ��ʹ�� HMM �´ʷ��ֹ���ʱ������Ч��

����ʾ����

```pycon
>>> print('/'.join(jieba.cut('����ŵ�post�н�����', HMM=False)))
���/�ŵ�/post/�н�/����/��
>>> jieba.suggest_freq(('��', '��'), True)
494
>>> print('/'.join(jieba.cut('����ŵ�post�н�����', HMM=False)))
���/�ŵ�/post/��/��/����/��
>>> print('/'.join(jieba.cut('��̨�С���ȷӦ�ò��ᱻ�п�', HMM=False)))
��/̨/��/��/��ȷ/Ӧ��/����/��/�п�
>>> jieba.suggest_freq('̨��', True)
69
>>> print('/'.join(jieba.cut('��̨�С���ȷӦ�ò��ᱻ�п�', HMM=False)))
��/̨��/��/��ȷ/Ӧ��/����/��/�п�
```

* "ͨ���û��Զ���ʵ�����ǿ�����������" --- https://github.com/fxsjy/jieba/issues/14

3. �ؼ�����ȡ
-------------
### ���� TF-IDF �㷨�Ĺؼ��ʳ�ȡ

`import jieba.analyse`

* jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())
  * sentence Ϊ����ȡ���ı�
  * topK Ϊ���ؼ��� TF/IDF Ȩ�����Ĺؼ��ʣ�Ĭ��ֵΪ 20
  * withWeight Ϊ�Ƿ�һ�����عؼ���Ȩ��ֵ��Ĭ��ֵΪ False
  * allowPOS ������ָ�����ԵĴʣ�Ĭ��ֵΪ�գ�����ɸѡ
* jieba.analyse.TFIDF(idf_path=None) �½� TFIDF ʵ����idf_path Ϊ IDF Ƶ���ļ�

����ʾ�� ���ؼ�����ȡ��

https://github.com/fxsjy/jieba/blob/master/test/extract_tags.py

�ؼ�����ȡ��ʹ�������ļ�Ƶ�ʣ�IDF���ı����Ͽ�����л����Զ������Ͽ��·��

* �÷��� jieba.analyse.set_idf_path(file_name) # file_nameΪ�Զ������Ͽ��·��
* �Զ������Ͽ�ʾ����https://github.com/fxsjy/jieba/blob/master/extra_dict/idf.txt.big
* �÷�ʾ����https://github.com/fxsjy/jieba/blob/master/test/extract_tags_idfpath.py

�ؼ�����ȡ��ʹ��ֹͣ�ʣ�Stop Words���ı����Ͽ�����л����Զ������Ͽ��·��

* �÷��� jieba.analyse.set_stop_words(file_name) # file_nameΪ�Զ������Ͽ��·��
* �Զ������Ͽ�ʾ����https://github.com/fxsjy/jieba/blob/master/extra_dict/stop_words.txt
* �÷�ʾ����https://github.com/fxsjy/jieba/blob/master/test/extract_tags_stop_words.py

�ؼ���һ�����عؼ���Ȩ��ֵʾ��

* �÷�ʾ����https://github.com/fxsjy/jieba/blob/master/test/extract_tags_with_weight.py

### ���� TextRank �㷨�Ĺؼ��ʳ�ȡ

* jieba.analyse.textrank(sentence, topK=20, withWeight=False, allowPOS=('ns', 'n', 'vn', 'v')) ֱ��ʹ�ã��ӿ���ͬ��ע��Ĭ�Ϲ��˴��ԡ�
* jieba.analyse.TextRank() �½��Զ��� TextRank ʵ��

�㷨���ģ� [TextRank: Bringing Order into Texts](http://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf)

#### ����˼��:

1. ������ȡ�ؼ��ʵ��ı����зִ�
2. �Թ̶����ڴ�С(Ĭ��Ϊ5��ͨ��span���Ե���)����֮��Ĺ��ֹ�ϵ������ͼ
3. ����ͼ�нڵ��PageRank��ע���������Ȩͼ

#### ʹ��ʾ��:

�� [test/demo.py](https://github.com/fxsjy/jieba/blob/master/test/demo.py)

4. ���Ա�ע
-----------
* `jieba.posseg.POSTokenizer(tokenizer=None)` �½��Զ���ִ�����`tokenizer` ������ָ���ڲ�ʹ�õ� `jieba.Tokenizer` �ִ�����`jieba.posseg.dt` ΪĬ�ϴ��Ա�ע�ִ�����
* ��ע���ӷִʺ�ÿ���ʵĴ��ԣ����ú� ictclas ���ݵı�Ƿ���
* ����jiebaĬ�Ϸִ�ģʽ���ṩpaddleģʽ�µĴ��Ա�ע���ܡ�paddleģʽ�����ӳټ��ط�ʽ��ͨ��enable_paddle()��װpaddlepaddle-tiny������import��ش��룻
* �÷�ʾ��

```pycon
>>> import jieba
>>> import jieba.posseg as pseg
>>> words = pseg.cut("�Ұ������찲��") #jiebaĬ��ģʽ
>>> jieba.enable_paddle() #����paddleģʽ�� 0.40��֮��ʼ֧�֣����ڰ汾��֧��
>>> words = pseg.cut("�Ұ������찲��",use_paddle=True) #paddleģʽ
>>> for word, flag in words:
...    print('%s %s' % (word, flag))
...
�� r
�� v
���� ns
�찲�� ns
```

paddleģʽ���Ա�ע��Ӧ�����£�

paddleģʽ���Ժ�ר������ǩ�������±����д��Ա�ǩ 24 ����Сд��ĸ����ר������ǩ 4 ������д��ĸ����

| ��ǩ | ����     | ��ǩ | ����     | ��ǩ | ����     | ��ǩ | ����     |
| ---- | -------- | ---- | -------- | ---- | -------- | ---- | -------- |
| n    | ��ͨ���� | f    | ��λ���� | s    | �������� | t    | ʱ��     |
| nr   | ����     | ns   | ����     | nt   | ������   | nw   | ��Ʒ��   |
| nz   | ����ר�� | v    | ��ͨ���� | vd   | ������   | vn   | ������   |
| a    | ���ݴ�   | ad   | ���δ�   | an   | ���δ�   | d    | ����     |
| m    | ������   | q    | ����     | r    | ����     | p    | ���     |
| c    | ����     | u    | ����     | xc   | ������� | w    | ������ |
| PER  | ����     | LOC  | ����     | ORG  | ������   | TIME | ʱ��     |


5. ���зִ�
-----------
* ԭ����Ŀ���ı����зָ��󣬰Ѹ����ı����䵽��� Python ���̲��зִʣ�Ȼ��鲢������Ӷ���÷ִ��ٶȵĿɹ�����
* ���� python �Դ��� multiprocessing ģ�飬Ŀǰ�ݲ�֧�� Windows
* �÷���
    * `jieba.enable_parallel(4)` # �������зִ�ģʽ������Ϊ���н�����
    * `jieba.disable_parallel()` # �رղ��зִ�ģʽ

* ���ӣ�https://github.com/fxsjy/jieba/blob/master/test/parallel/test_file.py

* ʵ�������� 4 �� 3.4GHz Linux �����ϣ��Խ�ӹȫ�����о�ȷ�ִʣ������ 1MB/s ���ٶȣ��ǵ����̰�� 3.3 ����

* **ע��**�����зִʽ�֧��Ĭ�Ϸִ��� `jieba.dt` �� `jieba.posseg.dt`��

6. Tokenize�����ش�����ԭ�ĵ���ֹλ��
----------------------------------
* ע�⣬�������ֻ���� unicode
* Ĭ��ģʽ

```python
result = jieba.tokenize(u'���ͷ�װ��Ʒ���޹�˾')
for tk in result:
    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))
```

```
word ����                start: 0                end:2
word ��װ                start: 2                end:4
word ��Ʒ                start: 4                end:6
word ���޹�˾            start: 6                end:10

```

* ����ģʽ

```python
result = jieba.tokenize(u'���ͷ�װ��Ʒ���޹�˾', mode='search')
for tk in result:
    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))
```

```
word ����                start: 0                end:2
word ��װ                start: 2                end:4
word ��Ʒ                start: 4                end:6
word ����                start: 6                end:8
word ��˾                start: 8                end:10
word ���޹�˾            start: 6                end:10
```


7. ChineseAnalyzer for Whoosh ��������
--------------------------------------------
* ���ã� `from jieba.analyse import ChineseAnalyzer`
* �÷�ʾ����https://github.com/fxsjy/jieba/blob/master/test/test_whoosh.py

8. �����зִ�
-------------------

ʹ��ʾ����`python -m jieba news.txt > cut_result.txt`

������ѡ����룩��

    ʹ��: python -m jieba [options] filename

    ��������н��档

    �̶�����:
      filename              �����ļ�

    ��ѡ����:
      -h, --help            ��ʾ�˰�����Ϣ���˳�
      -d [DELIM], --delimiter [DELIM]
                            ʹ�� DELIM �ָ������������Ĭ�ϵ�' / '��
                            ����ָ�� DELIM����ʹ��һ���ո�ָ���
      -p [DELIM], --pos [DELIM]
                            ���ô��Ա�ע�����ָ�� DELIM������ʹ���֮��
                            �����ָ��������� _ �ָ�
      -D DICT, --dict DICT  ʹ�� DICT ����Ĭ�ϴʵ�
      -u USER_DICT, --user-dict USER_DICT
                            ʹ�� USER_DICT ��Ϊ���Ӵʵ䣬��Ĭ�ϴʵ���Զ���ʵ����ʹ��
      -a, --cut-all         ȫģʽ�ִʣ���֧�ִ��Ա�ע��
      -n, --no-hmm          ��ʹ����������ɷ�ģ��
      -q, --quiet           �����������Ϣ�� STDERR
      -V, --version         ��ʾ�汾��Ϣ���˳�

    ���û��ָ���ļ�������ʹ�ñ�׼���롣

`--help` ѡ�������

    $> python -m jieba --help
    Jieba command line interface.

    positional arguments:
      filename              input file

    optional arguments:
      -h, --help            show this help message and exit
      -d [DELIM], --delimiter [DELIM]
                            use DELIM instead of ' / ' for word delimiter; or a
                            space if it is used without DELIM
      -p [DELIM], --pos [DELIM]
                            enable POS tagging; if DELIM is specified, use DELIM
                            instead of '_' for POS delimiter
      -D DICT, --dict DICT  use DICT as dictionary
      -u USER_DICT, --user-dict USER_DICT
                            use USER_DICT together with the default dictionary or
                            DICT (if specified)
      -a, --cut-all         full pattern cutting (ignored with POS tagging)
      -n, --no-hmm          don't use the Hidden Markov Model
      -q, --quiet           don't print loading messages to stderr
      -V, --version         show program's version number and exit

    If no filename specified, use STDIN instead.

�ӳټ��ػ���
------------

jieba �����ӳټ��أ�`import jieba` �� `jieba.Tokenizer()` �������������ʵ�ļ��أ�һ���б�Ҫ�ſ�ʼ���شʵ乹��ǰ׺�ֵ䡣��������ֹ���ʼ jieba��Ҳ�����ֶ���ʼ����

    import jieba
    jieba.initialize()  # �ֶ���ʼ������ѡ��


�� 0.28 ֮ǰ�İ汾�ǲ���ָ�����ʵ��·���ģ������ӳټ��ػ��ƺ�����Ըı����ʵ��·��:

    jieba.set_dictionary('data/dict.txt.big')

���ӣ� https://github.com/fxsjy/jieba/blob/master/test/test_change_dictpath.py

�����ʵ�
========
1. ռ���ڴ��С�Ĵʵ��ļ�
https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.small

2. ֧�ַ���ִʸ��õĴʵ��ļ�
https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.big

����������Ҫ�Ĵʵ䣬Ȼ�󸲸� jieba/dict.txt ���ɣ������� `jieba.set_dictionary('data/dict.txt.big')`

��������ʵ��
==========

��ͷִ� Java �汾
----------------
���ߣ�piaolingxue
��ַ��https://github.com/huaban/jieba-analysis

��ͷִ� C++ �汾
----------------
���ߣ�yanyiwu
��ַ��https://github.com/yanyiwu/cppjieba

��ͷִ� Rust �汾
----------------
���ߣ�messense, MnO2
��ַ��https://github.com/messense/jieba-rs

��ͷִ� Node.js �汾
----------------
���ߣ�yanyiwu
��ַ��https://github.com/yanyiwu/nodejieba

��ͷִ� Erlang �汾
----------------
���ߣ�falood
��ַ��https://github.com/falood/exjieba

��ͷִ� R �汾
----------------
���ߣ�qinwf
��ַ��https://github.com/qinwf/jiebaR

��ͷִ� iOS �汾
----------------
���ߣ�yanyiwu
��ַ��https://github.com/yanyiwu/iosjieba

��ͷִ� PHP �汾
----------------
���ߣ�fukuball
��ַ��https://github.com/fukuball/jieba-php

��ͷִ� .NET(C#) �汾
----------------
���ߣ�anderscui
��ַ��https://github.com/anderscui/jieba.NET/

��ͷִ� Go �汾
----------------

+ ����: wangbin ��ַ: https://github.com/wangbin/jiebago
+ ����: yanyiwu ��ַ: https://github.com/yanyiwu/gojieba

��ͷִ�Android�汾
------------------
+ ����   Dongliang.W  ��ַ��https://github.com/452896915/jieba-android


��������
=========
* https://github.com/baidu/lac   �ٶ����Ĵʷ��������ִ�+����+ר����ϵͳ
* https://github.com/baidu/AnyQ  �ٶ�FAQ�Զ��ʴ�ϵͳ
* https://github.com/baidu/Senta �ٶ����ʶ��ϵͳ

ϵͳ����
========
1. Solr: https://github.com/sing1ee/jieba-solr

�ִ��ٶ�
=========
* 1.5 MB / Second in Full Mode
* 400 KB / Second in Default Mode
* ���Ի���: Intel(R) Core(TM) i7-2600 CPU @ 3.4GHz����Χ�ǡ�.txt

��������
=========

## 1. ģ�͵�������������ɵģ�

����� https://github.com/fxsjy/jieba/issues/7

## 2. ��̨�С����Ǳ��гɡ�̨ �С������Լ����������

P(̨��) �� P(̨)��P(��)����̨�С���Ƶ����������ɴʸ��ʽϵ�

���������ǿ�Ƶ��ߴ�Ƶ

`jieba.add_word('̨��')` ���� `jieba.suggest_freq('̨��', True)`

## 3. ���������� ����Ӧ�ñ��гɡ����� ���� ���������Լ����������

���������ǿ�Ƶ��ʹ�Ƶ

`jieba.suggest_freq(('����', '����'), True)`

����ֱ��ɾ���ô� `jieba.del_word('��������')`

## 4. �г��˴ʵ���û�еĴ��Ч�������룿

����������ر��´ʷ���

`jieba.cut('����̫ʡ��', HMM=False)`
`jieba.cut('�����г���һ����ͽ', HMM=False)`

**������������**��https://github.com/fxsjy/jieba/issues?sort=updated&state=closed

�޶���ʷ
==========
https://github.com/fxsjy/jieba/blob/master/Changelog

--------------------

jieba
========
"Jieba" (Chinese for "to stutter") Chinese text segmentation: built to be the best Python Chinese word segmentation module.

Features
========
* Support three types of segmentation mode:

1. Accurate Mode attempts to cut the sentence into the most accurate segmentations, which is suitable for text analysis.
2. Full Mode gets all the possible words from the sentence. Fast but not accurate.
3. Search Engine Mode, based on the Accurate Mode, attempts to cut long words into several short words, which can raise the recall rate. Suitable for search engines.

* Supports Traditional Chinese
* Supports customized dictionaries
* MIT License


Online demo
=========
http://jiebademo.ap01.aws.af.cm/

(Powered by Appfog)

Usage
========
* Fully automatic installation: `easy_install jieba` or `pip install jieba`
* Semi-automatic installation: Download http://pypi.python.org/pypi/jieba/ , run `python setup.py install` after extracting.
* Manual installation: place the `jieba` directory in the current directory or python `site-packages` directory.
* `import jieba`.

Algorithm
========
* Based on a prefix dictionary structure to achieve efficient word graph scanning. Build a directed acyclic graph (DAG) for all possible word combinations.
* Use dynamic programming to find the most probable combination based on the word frequency.
* For unknown words, a HMM-based model is used with the Viterbi algorithm.

Main Functions
==============

1. Cut
--------
* The `jieba.cut` function accepts three input parameters: the first parameter is the string to be cut; the second parameter is `cut_all`, controlling the cut mode; the third parameter is to control whether to use the Hidden Markov Model.
* `jieba.cut_for_search` accepts two parameter: the string to be cut; whether to use the Hidden Markov Model. This will cut the sentence into short words suitable for search engines.
* The input string can be an unicode/str object, or a str/bytes object which is encoded in UTF-8 or GBK. Note that using GBK encoding is not recommended because it may be unexpectly decoded as UTF-8.
* `jieba.cut` and `jieba.cut_for_search` returns an generator, from which you can use a `for` loop to get the segmentation result (in unicode).
* `jieba.lcut` and `jieba.lcut_for_search` returns a list.
* `jieba.Tokenizer(dictionary=DEFAULT_DICT)` creates a new customized Tokenizer, which enables you to use different dictionaries at the same time. `jieba.dt` is the default Tokenizer, to which almost all global functions are mapped.


**Code example: segmentation**

```python
#encoding=utf-8
import jieba

seg_list = jieba.cut("�����������廪��ѧ", cut_all=True)
print("Full Mode: " + "/ ".join(seg_list))  # ȫģʽ

seg_list = jieba.cut("�����������廪��ѧ", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # Ĭ��ģʽ

seg_list = jieba.cut("�����������׺��д���")
print(", ".join(seg_list))

seg_list = jieba.cut_for_search("С��˶ʿ��ҵ���й���ѧԺ�������������ձ�������ѧ����")  # ��������ģʽ
print(", ".join(seg_list))
```

Output:

    [Full Mode]: ��/ ����/ ����/ �廪/ �廪��ѧ/ ����/ ��ѧ

    [Accurate Mode]: ��/ ����/ ����/ �廪��ѧ

    [Unknown Words Recognize] ��, ����, ��, ����, ����, ����    (In this case, "����" is not in the dictionary, but is identified by the Viterbi algorithm)

    [Search Engine Mode]�� С��, ˶ʿ, ��ҵ, ��, �й�, ��ѧ, ѧԺ, ��ѧԺ, �й���ѧԺ, ����, ������, ��, ��, �ձ�, ����, ��ѧ, �ձ�������ѧ, ����


2. Add a custom dictionary
----------------------------

### Load dictionary

* Developers can specify their own custom dictionary to be included in the jieba default dictionary. Jieba is able to identify new words, but you can add your own new words can ensure a higher accuracy.
* Usage�� `jieba.load_userdict(file_name)` # file_name is a file-like object or the path of the custom dictionary
* The dictionary format is the same as that of `dict.txt`: one word per line; each line is divided into three parts separated by a space: word, word frequency, POS tag. If `file_name` is a path or a file opened in binary mode, the dictionary must be UTF-8 encoded.
* The word frequency and POS tag can be omitted respectively. The word frequency will be filled with a suitable value if omitted.

**For example:**

```
���°� 3 i
�Ƽ��� 5
�P���� nz
̨��
```


* Change a Tokenizer's `tmp_dir` and `cache_file` to specify the path of the cache file, for using on a restricted file system.

* Example:

        �Ƽ��� 5
        ��С�� 2
        ���°� 3

        [Before]�� ��С�� / �� / ���� / �� / ���� / Ҳ / �� / �� / ���� / ���� / �� / ר�� /

        [After]������С�� / �� / ���°� / ���� / Ҳ / �� / �Ƽ��� / ���� / �� / ר�� /


### Modify dictionary

* Use `add_word(word, freq=None, tag=None)` and `del_word(word)` to modify the dictionary dynamically in programs.
* Use `suggest_freq(segment, tune=True)` to adjust the frequency of a single word so that it can (or cannot) be segmented.

* Note that HMM may affect the final result.

Example:

```pycon
>>> print('/'.join(jieba.cut('����ŵ�post�н�����', HMM=False)))
���/�ŵ�/post/�н�/����/��
>>> jieba.suggest_freq(('��', '��'), True)
494
>>> print('/'.join(jieba.cut('����ŵ�post�н�����', HMM=False)))
���/�ŵ�/post/��/��/����/��
>>> print('/'.join(jieba.cut('��̨�С���ȷӦ�ò��ᱻ�п�', HMM=False)))
��/̨/��/��/��ȷ/Ӧ��/����/��/�п�
>>> jieba.suggest_freq('̨��', True)
69
>>> print('/'.join(jieba.cut('��̨�С���ȷӦ�ò��ᱻ�п�', HMM=False)))
��/̨��/��/��ȷ/Ӧ��/����/��/�п�
```

3. Keyword Extraction
-----------------------
`import jieba.analyse`

* `jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())`
  * `sentence`: the text to be extracted
  * `topK`: return how many keywords with the highest TF/IDF weights. The default value is 20
  * `withWeight`: whether return TF/IDF weights with the keywords. The default value is False
  * `allowPOS`: filter words with which POSs are included. Empty for no filtering.
* `jieba.analyse.TFIDF(idf_path=None)` creates a new TFIDF instance, `idf_path` specifies IDF file path.

Example (keyword extraction)

https://github.com/fxsjy/jieba/blob/master/test/extract_tags.py

Developers can specify their own custom IDF corpus in jieba keyword extraction

* Usage�� `jieba.analyse.set_idf_path(file_name) # file_name is the path for the custom corpus`
* Custom Corpus Sample��https://github.com/fxsjy/jieba/blob/master/extra_dict/idf.txt.big
* Sample Code��https://github.com/fxsjy/jieba/blob/master/test/extract_tags_idfpath.py

Developers can specify their own custom stop words corpus in jieba keyword extraction

* Usage�� `jieba.analyse.set_stop_words(file_name) # file_name is the path for the custom corpus`
* Custom Corpus Sample��https://github.com/fxsjy/jieba/blob/master/extra_dict/stop_words.txt
* Sample Code��https://github.com/fxsjy/jieba/blob/master/test/extract_tags_stop_words.py

There's also a [TextRank](http://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf) implementation available.

Use: `jieba.analyse.textrank(sentence, topK=20, withWeight=False, allowPOS=('ns', 'n', 'vn', 'v'))`

Note that it filters POS by default.

`jieba.analyse.TextRank()` creates a new TextRank instance.

4. Part of Speech Tagging
-------------------------
* `jieba.posseg.POSTokenizer(tokenizer=None)` creates a new customized Tokenizer. `tokenizer` specifies the jieba.Tokenizer to internally use. `jieba.posseg.dt` is the default POSTokenizer.
* Tags the POS of each word after segmentation, using labels compatible with ictclas.
* Example:

```pycon
>>> import jieba.posseg as pseg
>>> words = pseg.cut("�Ұ������찲��")
>>> for w in words:
...    print('%s %s' % (w.word, w.flag))
...
�� r
�� v
���� ns
�찲�� ns
```

5. Parallel Processing
----------------------
* Principle: Split target text by line, assign the lines into multiple Python processes, and then merge the results, which is considerably faster.
* Based on the multiprocessing module of Python.
* Usage:
    * `jieba.enable_parallel(4)` # Enable parallel processing. The parameter is the number of processes.
    * `jieba.disable_parallel()` # Disable parallel processing.

* Example:
    https://github.com/fxsjy/jieba/blob/master/test/parallel/test_file.py

* Result: On a four-core 3.4GHz Linux machine, do accurate word segmentation on Complete Works of Jin Yong, and the speed reaches 1MB/s, which is 3.3 times faster than the single-process version.

* **Note** that parallel processing supports only default tokenizers, `jieba.dt` and `jieba.posseg.dt`.

6. Tokenize: return words with position
----------------------------------------
* The input must be unicode
* Default mode

```python
result = jieba.tokenize(u'���ͷ�װ��Ʒ���޹�˾')
for tk in result:
    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))
```

```
word ����                start: 0                end:2
word ��װ                start: 2                end:4
word ��Ʒ                start: 4                end:6
word ���޹�˾            start: 6                end:10

```

* Search mode

```python
result = jieba.tokenize(u'���ͷ�װ��Ʒ���޹�˾',mode='search')
for tk in result:
    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))
```

```
word ����                start: 0                end:2
word ��װ                start: 2                end:4
word ��Ʒ                start: 4                end:6
word ����                start: 6                end:8
word ��˾                start: 8                end:10
word ���޹�˾            start: 6                end:10
```


7. ChineseAnalyzer for Whoosh
-------------------------------
* `from jieba.analyse import ChineseAnalyzer`
* Example: https://github.com/fxsjy/jieba/blob/master/test/test_whoosh.py

8. Command Line Interface
--------------------------------

    $> python -m jieba --help
    Jieba command line interface.

    positional arguments:
      filename              input file

    optional arguments:
      -h, --help            show this help message and exit
      -d [DELIM], --delimiter [DELIM]
                            use DELIM instead of ' / ' for word delimiter; or a
                            space if it is used without DELIM
      -p [DELIM], --pos [DELIM]
                            enable POS tagging; if DELIM is specified, use DELIM
                            instead of '_' for POS delimiter
      -D DICT, --dict DICT  use DICT as dictionary
      -u USER_DICT, --user-dict USER_DICT
                            use USER_DICT together with the default dictionary or
                            DICT (if specified)
      -a, --cut-all         full pattern cutting (ignored with POS tagging)
      -n, --no-hmm          don't use the Hidden Markov Model
      -q, --quiet           don't print loading messages to stderr
      -V, --version         show program's version number and exit

    If no filename specified, use STDIN instead.

Initialization
---------------
By default, Jieba don't build the prefix dictionary unless it's necessary. This takes 1-3 seconds, after which it is not initialized again. If you want to initialize Jieba manually, you can call:

    import jieba
    jieba.initialize()  # (optional)

You can also specify the dictionary (not supported before version 0.28) :

    jieba.set_dictionary('data/dict.txt.big')


Using Other Dictionaries
===========================

It is possible to use your own dictionary with Jieba, and there are also two dictionaries ready for download:

1. A smaller dictionary for a smaller memory footprint:
https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.small

2. There is also a bigger dictionary that has better support for traditional Chinese (���w):
https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.big

By default, an in-between dictionary is used, called `dict.txt` and included in the distribution.

In either case, download the file you want, and then call `jieba.set_dictionary('data/dict.txt.big')` or just replace the existing `dict.txt`.

Segmentation speed
=========
* 1.5 MB / Second in Full Mode
* 400 KB / Second in Default Mode
* Test Env: Intel(R) Core(TM) i7-2600 CPU @ 3.4GHz����Χ�ǡ�.txt
