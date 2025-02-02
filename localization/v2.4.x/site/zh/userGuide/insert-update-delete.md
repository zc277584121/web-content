---
id: insert-update-delete.md
summary: 本指南将向您介绍 Collections 中的数据操作符操作，包括插入、向上插入和删除。
title: 插入、上插和删除
---
<h1 id="Insert-Upsert--Delete" class="common-anchor-header">插入、上插和删除<button data-href="#Insert-Upsert--Delete" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h1><p>本指南将向您介绍 Collections 中的数据操作符，包括插入、向上插入和删除。</p>
<h2 id="Before-you-start" class="common-anchor-header">开始之前<button data-href="#Before-you-start" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><ul>
<li><p>您已安装了所选的 SDK。要安装 SDK，请参阅<a href="https://milvus.io/docs/install-pymilvus.md">安装 SDK</a>。</p></li>
<li><p>您已创建了一个 Collections。要创建一个<a href="/docs/zh/manage-collections.md">Collections</a>，请参阅<a href="/docs/zh/manage-collections.md">管理 Collections</a>。</p></li>
<li><p>要插入大量数据，建议使用<a href="https://milvus.io/api-reference/pymilvus/v2.4.x/DataImport/LocalBulkWriter/LocalBulkWriter.md">数据导入</a>。</p></li>
</ul>
<h2 id="Overview" class="common-anchor-header">概述<button data-href="#Overview" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>实体，在 Milvus 集合的上下文中，是集合中的一个单一的、可识别的实例。它代表特定类别中的一个独特成员，无论是图书馆中的一本书、基因组中的一个基因，还是其他任何可识别的实体。</p>
<p>Collections 中的实体共享一组共同的属性（称为 Schema），这些属性概述了每个实体必须遵守的结构，包括字段名称、数据类型和其他限制条件。</p>
<p>要将实体成功插入 Collections，所提供的数据必须包含目标 Collections 的所有 Schema 定义字段。此外，只有启用了动态字段，才能包含非 Schema 定义的字段。有关详细信息，请参阅<a href="/docs/zh/enable-dynamic-field.md">启用动态字段</a>。</p>
<h2 id="Preparations" class="common-anchor-header">准备工作<button data-href="#Preparations" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>下面的代码片段重新利用了现有代码，以建立与 Milvus 集群的连接，并快速设置一个 Collections。</p>
<div class="language-python">
<p>对于准备工作，使用 <a href="https://milvus.io/api-reference/pymilvus/v2.4.x/MilvusClient/Client/MilvusClient.md"><code translate="no">MilvusClient</code></a>连接到 Milvus 服务器，并使用 <a href="https://milvus.io/api-reference/pymilvus/v2.4.x/MilvusClient/Collections/create_collection.md"><code translate="no">create_collection()</code></a>以快速设置模式创建 Collections。</p>
</div>
<div class="language-java">
<p>对于准备工作，使用 <a href="https://milvus.io/api-reference/java/v2.4.x/v2/Client/MilvusClientV2.md"><code translate="no">MilvusClientV2</code></a>连接到 Milvus 服务器，并使用 <a href="https://milvus.io/api-reference/java/v2.4.x/v2/Collections/createCollection.md"><code translate="no">createCollection()</code></a>以快速设置模式创建 Collections。</p>
</div>
<div class="language-javascript">
<p>对于准备工作，使用 <a href="https://milvus.io/api-reference/node/v2.4.x/Client/MilvusClient.md"><code translate="no">MilvusClient</code></a>连接到 Milvus 服务器，并使用 <a href="https://milvus.io/api-reference/node/v2.4.x/Collections/createCollection.md"><code translate="no">createCollection()</code></a>以快速设置模式创建 Collections。</p>
</div>
<div class="multipleCode">
   <a href="#python">Python </a> <a href="#java">Java</a> <a href="#javascript">Node.js</a></div>
<pre><code translate="no" class="language-python"><span class="hljs-keyword">from</span> pymilvus <span class="hljs-keyword">import</span> MilvusClient

<span class="hljs-comment"># 1. Set up a Milvus client</span>
client = MilvusClient(
    uri=<span class="hljs-string">&quot;http://localhost:19530&quot;</span>
)

<span class="hljs-comment"># 2. Create a collection</span>
client.create_collection(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    dimension=<span class="hljs-number">5</span>,
    metric_type=<span class="hljs-string">&quot;IP&quot;</span>
)
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java"><span class="hljs-keyword">import</span> io.milvus.v2.client.ConnectConfig;
<span class="hljs-keyword">import</span> io.milvus.v2.client.MilvusClientV2;
<span class="hljs-keyword">import</span> io.milvus.v2.service.collection.request.CreateCollectionReq;

<span class="hljs-type">String</span> <span class="hljs-variable">CLUSTER_ENDPOINT</span> <span class="hljs-operator">=</span> <span class="hljs-string">&quot;http://localhost:19530&quot;</span>;

<span class="hljs-comment">// 1. Connect to Milvus server</span>
<span class="hljs-type">ConnectConfig</span> <span class="hljs-variable">connectConfig</span> <span class="hljs-operator">=</span> ConnectConfig.builder()
    .uri(CLUSTER_ENDPOINT)
    .build();

<span class="hljs-type">MilvusClientV2</span> <span class="hljs-variable">client</span> <span class="hljs-operator">=</span> <span class="hljs-keyword">new</span> <span class="hljs-title class_">MilvusClientV2</span>(connectConfig);

<span class="hljs-comment">// 2. Create a collection in quick setup mode</span>
<span class="hljs-type">CreateCollectionReq</span> <span class="hljs-variable">quickSetupReq</span> <span class="hljs-operator">=</span> CreateCollectionReq.builder()
    .collectionName(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .dimension(<span class="hljs-number">5</span>)
    .metricType(<span class="hljs-string">&quot;IP&quot;</span>)
    .build();

client.createCollection(quickSetupReq);
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript"><span class="hljs-keyword">const</span> { <span class="hljs-title class_">MilvusClient</span>, <span class="hljs-title class_">DataType</span>, sleep } = <span class="hljs-built_in">require</span>(<span class="hljs-string">&quot;@zilliz/milvus2-sdk-node&quot;</span>)

<span class="hljs-keyword">const</span> address = <span class="hljs-string">&quot;http://localhost:19530&quot;</span>

<span class="hljs-comment">// 1. Set up a Milvus Client</span>
client = <span class="hljs-keyword">new</span> <span class="hljs-title class_">MilvusClient</span>({address});

<span class="hljs-comment">// 2. Create a collection in quick setup mode</span>
<span class="hljs-keyword">await</span> client.<span class="hljs-title function_">createCollection</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">dimension</span>: <span class="hljs-number">5</span>,
    <span class="hljs-attr">metric_type</span>: <span class="hljs-string">&quot;IP&quot;</span>
});  
<button class="copy-code-btn"></button></code></pre>
<div class="admonition note">
<p><b>注释</b></p>
<p>上述代码生成的 Collections 只包含两个字段：<code translate="no">id</code> （作为主键）和<code translate="no">vector</code> （作为向量字段），默认启用<code translate="no">auto_id</code> 和<code translate="no">enable_dynamic_field</code> 设置。插入数据时、</p>
<ul>
<li><p>无需在要插入的数据中包含<strong>id</strong>，因为主字段会在插入数据时自动递增。</p></li>
<li><p>非 Schema 定义的字段将以键值对的形式保存在名为<strong>$meta</strong> 的预留 JSON 字段中。</p></li>
</ul>
</div>
<h2 id="Insert-entities" class="common-anchor-header">插入实体<button data-href="#Insert-entities" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>要插入实体，需要将数据组织成一个字典列表，其中每个字典代表一个实体。每个字典都包含与目标 Collections 中预定义字段和动态字段相对应的键。</p>
<div class="language-python">
<p>要将实体插入 Collections，请使用 <a href="https://milvus.io/api-reference/pymilvus/v2.4.x/MilvusClient/Vector/insert.md"><code translate="no">insert()</code></a>方法。</p>
</div>
<div class="language-java">
<p>要将实体插入 Collections，请使用 <a href="https://milvus.io/api-reference/java/v2.4.x/v2/Vector/insert.md"><code translate="no">insert()</code></a>方法。</p>
</div>
<div class="language-javascript">
<p>要将实体插入 Collections，请使用 <a href="https://milvus.io/api-reference/node/v2.4.x/Vector/insert.md"><code translate="no">insert()</code></a>方法。</p>
</div>
<div class="multipleCode">
   <a href="#python">Python </a> <a href="#java">Java</a> <a href="#javascript">Node.js</a></div>
<pre><code translate="no" class="language-python"><span class="hljs-comment"># 3. Insert some data</span>
data=[
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">0</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.3580376395471989</span>, -<span class="hljs-number">0.6023495712049978</span>, <span class="hljs-number">0.18414012509913835</span>, -<span class="hljs-number">0.26286205330961354</span>, <span class="hljs-number">0.9029438446296592</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;pink_8682&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">1</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.19886812562848388</span>, <span class="hljs-number">0.06023560599112088</span>, <span class="hljs-number">0.6976963061752597</span>, <span class="hljs-number">0.2614474506242501</span>, <span class="hljs-number">0.838729485096104</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_7025&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">2</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.43742130801983836</span>, -<span class="hljs-number">0.5597502546264526</span>, <span class="hljs-number">0.6457887650909682</span>, <span class="hljs-number">0.7894058910881185</span>, <span class="hljs-number">0.20785793220625592</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_6781&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">3</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.3172005263489739</span>, <span class="hljs-number">0.9719044792798428</span>, -<span class="hljs-number">0.36981146090600725</span>, -<span class="hljs-number">0.4860894583077995</span>, <span class="hljs-number">0.95791889146345</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;pink_9298&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">4</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.4452349528804562</span>, -<span class="hljs-number">0.8757026943054742</span>, <span class="hljs-number">0.8220779437047674</span>, <span class="hljs-number">0.46406290649483184</span>, <span class="hljs-number">0.30337481143159106</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_4794&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">5</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.985825131989184</span>, -<span class="hljs-number">0.8144651566660419</span>, <span class="hljs-number">0.6299267002202009</span>, <span class="hljs-number">0.1206906911183383</span>, -<span class="hljs-number">0.1446277761879955</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;yellow_4222&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">6</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.8371977790571115</span>, -<span class="hljs-number">0.015764369584852833</span>, -<span class="hljs-number">0.31062937026679327</span>, -<span class="hljs-number">0.562666951622192</span>, -<span class="hljs-number">0.8984947637863987</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_9392&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">7</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.33445148015177995</span>, -<span class="hljs-number">0.2567135004164067</span>, <span class="hljs-number">0.8987539745369246</span>, <span class="hljs-number">0.9402995886420709</span>, <span class="hljs-number">0.5378064918413052</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;grey_8510&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">8</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.39524717779832685</span>, <span class="hljs-number">0.4000257286739164</span>, -<span class="hljs-number">0.5890507376891594</span>, -<span class="hljs-number">0.8650502298996872</span>, -<span class="hljs-number">0.6140360785406336</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;white_9381&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">9</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.5718280481994695</span>, <span class="hljs-number">0.24070317428066512</span>, -<span class="hljs-number">0.3737913482606834</span>, -<span class="hljs-number">0.06726932177492717</span>, -<span class="hljs-number">0.6980531615588608</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;purple_4976&quot;</span>}
]

res = client.insert(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    data=data
)

<span class="hljs-built_in">print</span>(res)

<span class="hljs-comment"># Output</span>
<span class="hljs-comment">#</span>
<span class="hljs-comment"># {</span>
<span class="hljs-comment">#     &quot;insert_count&quot;: 10,</span>
<span class="hljs-comment">#     &quot;ids&quot;: [</span>
<span class="hljs-comment">#         0,</span>
<span class="hljs-comment">#         1,</span>
<span class="hljs-comment">#         2,</span>
<span class="hljs-comment">#         3,</span>
<span class="hljs-comment">#         4,</span>
<span class="hljs-comment">#         5,</span>
<span class="hljs-comment">#         6,</span>
<span class="hljs-comment">#         7,</span>
<span class="hljs-comment">#         8,</span>
<span class="hljs-comment">#         9</span>
<span class="hljs-comment">#     ]</span>
<span class="hljs-comment"># }</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java"><span class="hljs-keyword">import</span> java.<span class="hljs-property">util</span>.<span class="hljs-property">Arrays</span>;
<span class="hljs-keyword">import</span> java.<span class="hljs-property">util</span>.<span class="hljs-property">List</span>;
<span class="hljs-keyword">import</span> java.<span class="hljs-property">util</span>.<span class="hljs-property">Map</span>;
<span class="hljs-keyword">import</span> io.<span class="hljs-property">milvus</span>.<span class="hljs-property">v2</span>.<span class="hljs-property">service</span>.<span class="hljs-property">vector</span>.<span class="hljs-property">request</span>.<span class="hljs-property">InsertReq</span>;
<span class="hljs-keyword">import</span> io.<span class="hljs-property">milvus</span>.<span class="hljs-property">v2</span>.<span class="hljs-property">service</span>.<span class="hljs-property">vector</span>.<span class="hljs-property">response</span>.<span class="hljs-property">InsertResp</span>;

<span class="hljs-comment">// 3. Insert some data</span>
<span class="hljs-title class_">List</span>&lt;<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>&gt; data = <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 0L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>3580376395471989f, -<span class="hljs-number">0.</span>6023495712049978f, <span class="hljs-number">0.</span>18414012509913835f, -<span class="hljs-number">0.</span>26286205330961354f, <span class="hljs-number">0.</span>9029438446296592f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;pink_8682&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 1L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>19886812562848388f, <span class="hljs-number">0.</span>06023560599112088f, <span class="hljs-number">0.</span>6976963061752597f, <span class="hljs-number">0.</span>2614474506242501f, <span class="hljs-number">0.</span>838729485096104f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_7025&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 2L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>43742130801983836f, -<span class="hljs-number">0.</span>5597502546264526f, <span class="hljs-number">0.</span>6457887650909682f, <span class="hljs-number">0.</span>7894058910881185f, <span class="hljs-number">0.</span>20785793220625592f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_6781&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 3L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>3172005263489739f, <span class="hljs-number">0.</span>9719044792798428f, -<span class="hljs-number">0.</span>36981146090600725f, -<span class="hljs-number">0.</span>4860894583077995f, <span class="hljs-number">0.</span>95791889146345f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;pink_9298&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 4L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>4452349528804562f, -<span class="hljs-number">0.</span>8757026943054742f, <span class="hljs-number">0.</span>8220779437047674f, <span class="hljs-number">0.</span>46406290649483184f, <span class="hljs-number">0.</span>30337481143159106f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_4794&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 5L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>985825131989184f, -<span class="hljs-number">0.</span>8144651566660419f, <span class="hljs-number">0.</span>6299267002202009f, <span class="hljs-number">0.</span>1206906911183383f, -<span class="hljs-number">0.</span>1446277761879955f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;yellow_4222&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 6L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>8371977790571115f, -<span class="hljs-number">0.</span>015764369584852833f, -<span class="hljs-number">0.</span>31062937026679327f, -<span class="hljs-number">0.</span>562666951622192f, -<span class="hljs-number">0.</span>8984947637863987f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_9392&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 7L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>33445148015177995f, -<span class="hljs-number">0.</span>2567135004164067f, <span class="hljs-number">0.</span>8987539745369246f, <span class="hljs-number">0.</span>9402995886420709f, <span class="hljs-number">0.</span>5378064918413052f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;grey_8510&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 8L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>39524717779832685f, <span class="hljs-number">0.</span>4000257286739164f, -<span class="hljs-number">0.</span>5890507376891594f, -<span class="hljs-number">0.</span>8650502298996872f, -<span class="hljs-number">0.</span>6140360785406336f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;white_9381&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 9L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>5718280481994695f, <span class="hljs-number">0.</span>24070317428066512f, -<span class="hljs-number">0.</span>3737913482606834f, -<span class="hljs-number">0.</span>06726932177492717f, -<span class="hljs-number">0.</span>6980531615588608f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;purple_4976&quot;</span>))
);

<span class="hljs-title class_">InsertReq</span> insertReq = <span class="hljs-title class_">InsertReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">data</span>(data)
    .<span class="hljs-title function_">build</span>();

<span class="hljs-title class_">InsertResp</span> insertResp = client.<span class="hljs-title function_">insert</span>(insertReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(insertResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;insertCnt&quot;: 10}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript"><span class="hljs-comment">// 3. Insert some data</span>

<span class="hljs-keyword">var</span> data = [
    {<span class="hljs-attr">id</span>: <span class="hljs-number">0</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.3580376395471989</span>, -<span class="hljs-number">0.6023495712049978</span>, <span class="hljs-number">0.18414012509913835</span>, -<span class="hljs-number">0.26286205330961354</span>, <span class="hljs-number">0.9029438446296592</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;pink_8682&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">1</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.19886812562848388</span>, <span class="hljs-number">0.06023560599112088</span>, <span class="hljs-number">0.6976963061752597</span>, <span class="hljs-number">0.2614474506242501</span>, <span class="hljs-number">0.838729485096104</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;red_7025&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">2</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.43742130801983836</span>, -<span class="hljs-number">0.5597502546264526</span>, <span class="hljs-number">0.6457887650909682</span>, <span class="hljs-number">0.7894058910881185</span>, <span class="hljs-number">0.20785793220625592</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;orange_6781&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">3</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.3172005263489739</span>, <span class="hljs-number">0.9719044792798428</span>, -<span class="hljs-number">0.36981146090600725</span>, -<span class="hljs-number">0.4860894583077995</span>, <span class="hljs-number">0.95791889146345</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;pink_9298&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">4</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.4452349528804562</span>, -<span class="hljs-number">0.8757026943054742</span>, <span class="hljs-number">0.8220779437047674</span>, <span class="hljs-number">0.46406290649483184</span>, <span class="hljs-number">0.30337481143159106</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;red_4794&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">5</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.985825131989184</span>, -<span class="hljs-number">0.8144651566660419</span>, <span class="hljs-number">0.6299267002202009</span>, <span class="hljs-number">0.1206906911183383</span>, -<span class="hljs-number">0.1446277761879955</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;yellow_4222&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">6</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.8371977790571115</span>, -<span class="hljs-number">0.015764369584852833</span>, -<span class="hljs-number">0.31062937026679327</span>, -<span class="hljs-number">0.562666951622192</span>, -<span class="hljs-number">0.8984947637863987</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;red_9392&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">7</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.33445148015177995</span>, -<span class="hljs-number">0.2567135004164067</span>, <span class="hljs-number">0.8987539745369246</span>, <span class="hljs-number">0.9402995886420709</span>, <span class="hljs-number">0.5378064918413052</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;grey_8510&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">8</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.39524717779832685</span>, <span class="hljs-number">0.4000257286739164</span>, -<span class="hljs-number">0.5890507376891594</span>, -<span class="hljs-number">0.8650502298996872</span>, -<span class="hljs-number">0.6140360785406336</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;white_9381&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">9</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.5718280481994695</span>, <span class="hljs-number">0.24070317428066512</span>, -<span class="hljs-number">0.3737913482606834</span>, -<span class="hljs-number">0.06726932177492717</span>, -<span class="hljs-number">0.6980531615588608</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;purple_4976&quot;</span>}        
]

<span class="hljs-keyword">var</span> res = <span class="hljs-keyword">await</span> client.<span class="hljs-title function_">insert</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">data</span>: data,
})

<span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res.<span class="hljs-property">insert_cnt</span>)

<span class="hljs-comment">// Output</span>
<span class="hljs-comment">// </span>
<span class="hljs-comment">// 10</span>
<span class="hljs-comment">// </span>
<button class="copy-code-btn"></button></code></pre>
<h3 id="Insert-into-partitions" class="common-anchor-header">插入分区</h3><p>要将数据插入特定分区，可在插入请求中指定分区名称，如下所示：</p>
<div class="multipleCode">
   <a href="#python">Python </a> <a href="#java">Java</a> <a href="#javascript">Node.js</a></div>
<pre><code translate="no" class="language-python"><span class="hljs-comment"># 4. Insert some more data into a specific partition</span>
data=[
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">10</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.5570353903748935</span>, -<span class="hljs-number">0.8997887893201304</span>, -<span class="hljs-number">0.7123782431855732</span>, -<span class="hljs-number">0.6298990746450119</span>, <span class="hljs-number">0.6699215060604258</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_1202&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">11</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.6319019033373907</span>, <span class="hljs-number">0.6821488267878275</span>, <span class="hljs-number">0.8552303045704168</span>, <span class="hljs-number">0.36929791364943054</span>, -<span class="hljs-number">0.14152860714878068</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;blue_4150&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">12</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.9483947484855766</span>, -<span class="hljs-number">0.32294203351925344</span>, <span class="hljs-number">0.9759290319978025</span>, <span class="hljs-number">0.8262982148666174</span>, -<span class="hljs-number">0.8351194181285713</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_4590&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">13</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.5449109892498731</span>, <span class="hljs-number">0.043511240563786524</span>, -<span class="hljs-number">0.25105249484790804</span>, -<span class="hljs-number">0.012030655265886425</span>, -<span class="hljs-number">0.0010987671273892108</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;pink_9619&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">14</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.6603339372951424</span>, -<span class="hljs-number">0.10866551787442225</span>, -<span class="hljs-number">0.9435597754324891</span>, <span class="hljs-number">0.8230244263466688</span>, -<span class="hljs-number">0.7986720938400362</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_4863&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">15</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.8825129181091456</span>, -<span class="hljs-number">0.9204557711667729</span>, -<span class="hljs-number">0.935350065513425</span>, <span class="hljs-number">0.5484069690287079</span>, <span class="hljs-number">0.24448151140671204</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_7984&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">16</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.6285586391568163</span>, <span class="hljs-number">0.5389064528263487</span>, -<span class="hljs-number">0.3163366239905099</span>, <span class="hljs-number">0.22036279378888013</span>, <span class="hljs-number">0.15077052220816167</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;blue_9010&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">17</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.20151825016059233</span>, -<span class="hljs-number">0.905239387635804</span>, <span class="hljs-number">0.6749305353372479</span>, -<span class="hljs-number">0.7324272081377843</span>, -<span class="hljs-number">0.33007998971889263</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;blue_4521&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">18</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.2432286610792349</span>, <span class="hljs-number">0.01785636564206139</span>, -<span class="hljs-number">0.651356982731391</span>, -<span class="hljs-number">0.35848148851027895</span>, -<span class="hljs-number">0.7387383128324057</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_2529&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">19</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.055512329053363674</span>, <span class="hljs-number">0.7100266349039421</span>, <span class="hljs-number">0.4956956543575197</span>, <span class="hljs-number">0.24541352586717702</span>, <span class="hljs-number">0.4209030729923515</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_9437&quot;</span>}
]

client.create_partition(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    partition_name=<span class="hljs-string">&quot;partitionA&quot;</span>
)

res = client.insert(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    data=data,
    partition_name=<span class="hljs-string">&quot;partitionA&quot;</span>
)

<span class="hljs-built_in">print</span>(res)

<span class="hljs-comment"># Output</span>
<span class="hljs-comment">#</span>
<span class="hljs-comment"># {</span>
<span class="hljs-comment">#     &quot;insert_count&quot;: 10,</span>
<span class="hljs-comment">#     &quot;ids&quot;: [</span>
<span class="hljs-comment">#         10,</span>
<span class="hljs-comment">#         11,</span>
<span class="hljs-comment">#         12,</span>
<span class="hljs-comment">#         13,</span>
<span class="hljs-comment">#         14,</span>
<span class="hljs-comment">#         15,</span>
<span class="hljs-comment">#         16,</span>
<span class="hljs-comment">#         17,</span>
<span class="hljs-comment">#         18,</span>
<span class="hljs-comment">#         19</span>
<span class="hljs-comment">#     ]</span>
<span class="hljs-comment"># }</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java"><span class="hljs-comment">// 4. Insert some more data into a specific partition</span>
data = <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 10L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>5570353903748935f, -<span class="hljs-number">0.</span>8997887893201304f, -<span class="hljs-number">0.</span>7123782431855732f, -<span class="hljs-number">0.</span>6298990746450119f, <span class="hljs-number">0.</span>6699215060604258f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_1202&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 11L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>6319019033373907f, <span class="hljs-number">0.</span>6821488267878275f, <span class="hljs-number">0.</span>8552303045704168f, <span class="hljs-number">0.</span>36929791364943054f, -<span class="hljs-number">0.</span>14152860714878068f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;blue_4150&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 12L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>9483947484855766f, -<span class="hljs-number">0.</span>32294203351925344f, <span class="hljs-number">0.</span>9759290319978025f, <span class="hljs-number">0.</span>8262982148666174f, -<span class="hljs-number">0.</span>8351194181285713f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_4590&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 13L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>5449109892498731f, <span class="hljs-number">0.</span>043511240563786524f, -<span class="hljs-number">0.</span>25105249484790804f, -<span class="hljs-number">0.</span>012030655265886425f, -<span class="hljs-number">0.</span>0010987671273892108f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;pink_9619&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 14L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>6603339372951424f, -<span class="hljs-number">0.</span>10866551787442225f, -<span class="hljs-number">0.</span>9435597754324891f, <span class="hljs-number">0.</span>8230244263466688f, -<span class="hljs-number">0.</span>7986720938400362f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_4863&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 15L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>8825129181091456f, -<span class="hljs-number">0.</span>9204557711667729f, -<span class="hljs-number">0.</span>935350065513425f, <span class="hljs-number">0.</span>5484069690287079f, <span class="hljs-number">0.</span>24448151140671204f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_7984&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 16L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>6285586391568163f, <span class="hljs-number">0.</span>5389064528263487f, -<span class="hljs-number">0.</span>3163366239905099f, <span class="hljs-number">0.</span>22036279378888013f, <span class="hljs-number">0.</span>15077052220816167f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;blue_9010&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 17L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>20151825016059233f, -<span class="hljs-number">0.</span>905239387635804f, <span class="hljs-number">0.</span>6749305353372479f, -<span class="hljs-number">0.</span>7324272081377843f, -<span class="hljs-number">0.</span>33007998971889263f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;blue_4521&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 18L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>2432286610792349f, <span class="hljs-number">0.</span>01785636564206139f, -<span class="hljs-number">0.</span>651356982731391f, -<span class="hljs-number">0.</span>35848148851027895f, -<span class="hljs-number">0.</span>7387383128324057f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_2529&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 19L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>055512329053363674f, <span class="hljs-number">0.</span>7100266349039421f, <span class="hljs-number">0.</span>4956956543575197f, <span class="hljs-number">0.</span>24541352586717702f, <span class="hljs-number">0.</span>4209030729923515f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_9437&quot;</span>))
);

<span class="hljs-title class_">CreatePartitionReq</span> createPartitionReq = <span class="hljs-title class_">CreatePartitionReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">partitionName</span>(<span class="hljs-string">&quot;partitionA&quot;</span>)
    .<span class="hljs-title function_">build</span>();

client.<span class="hljs-title function_">createPartition</span>(createPartitionReq);

insertReq = <span class="hljs-title class_">InsertReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">data</span>(data)
    .<span class="hljs-title function_">partitionName</span>(<span class="hljs-string">&quot;partitionA&quot;</span>)
    .<span class="hljs-title function_">build</span>();

insertResp = client.<span class="hljs-title function_">insert</span>(insertReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(insertResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;insertCnt&quot;: 10}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript"><span class="hljs-comment">// 4. Insert some more data into a specific partition</span>
data = [
    {<span class="hljs-attr">id</span>: <span class="hljs-number">10</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.5570353903748935</span>, -<span class="hljs-number">0.8997887893201304</span>, -<span class="hljs-number">0.7123782431855732</span>, -<span class="hljs-number">0.6298990746450119</span>, <span class="hljs-number">0.6699215060604258</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;red_1202&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">11</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.6319019033373907</span>, <span class="hljs-number">0.6821488267878275</span>, <span class="hljs-number">0.8552303045704168</span>, <span class="hljs-number">0.36929791364943054</span>, -<span class="hljs-number">0.14152860714878068</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;blue_4150&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">12</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.9483947484855766</span>, -<span class="hljs-number">0.32294203351925344</span>, <span class="hljs-number">0.9759290319978025</span>, <span class="hljs-number">0.8262982148666174</span>, -<span class="hljs-number">0.8351194181285713</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;orange_4590&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">13</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.5449109892498731</span>, <span class="hljs-number">0.043511240563786524</span>, -<span class="hljs-number">0.25105249484790804</span>, -<span class="hljs-number">0.012030655265886425</span>, -<span class="hljs-number">0.0010987671273892108</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;pink_9619&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">14</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.6603339372951424</span>, -<span class="hljs-number">0.10866551787442225</span>, -<span class="hljs-number">0.9435597754324891</span>, <span class="hljs-number">0.8230244263466688</span>, -<span class="hljs-number">0.7986720938400362</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;orange_4863&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">15</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.8825129181091456</span>, -<span class="hljs-number">0.9204557711667729</span>, -<span class="hljs-number">0.935350065513425</span>, <span class="hljs-number">0.5484069690287079</span>, <span class="hljs-number">0.24448151140671204</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;orange_7984&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">16</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.6285586391568163</span>, <span class="hljs-number">0.5389064528263487</span>, -<span class="hljs-number">0.3163366239905099</span>, <span class="hljs-number">0.22036279378888013</span>, <span class="hljs-number">0.15077052220816167</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;blue_9010&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">17</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.20151825016059233</span>, -<span class="hljs-number">0.905239387635804</span>, <span class="hljs-number">0.6749305353372479</span>, -<span class="hljs-number">0.7324272081377843</span>, -<span class="hljs-number">0.33007998971889263</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;blue_4521&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">18</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.2432286610792349</span>, <span class="hljs-number">0.01785636564206139</span>, -<span class="hljs-number">0.651356982731391</span>, -<span class="hljs-number">0.35848148851027895</span>, -<span class="hljs-number">0.7387383128324057</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;orange_2529&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">19</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.055512329053363674</span>, <span class="hljs-number">0.7100266349039421</span>, <span class="hljs-number">0.4956956543575197</span>, <span class="hljs-number">0.24541352586717702</span>, <span class="hljs-number">0.4209030729923515</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;red_9437&quot;</span>}
]

<span class="hljs-keyword">await</span> client.<span class="hljs-title function_">createPartition</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">partition_name</span>: <span class="hljs-string">&quot;partitionA&quot;</span>
})

res = <span class="hljs-keyword">await</span> client.<span class="hljs-title function_">insert</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">data</span>: data,
    <span class="hljs-attr">partition_name</span>: <span class="hljs-string">&quot;partitionA&quot;</span>
})

<span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res.<span class="hljs-property">insert_cnt</span>)

<span class="hljs-comment">// Output</span>
<span class="hljs-comment">// </span>
<span class="hljs-comment">// 10</span>
<span class="hljs-comment">// </span>
<button class="copy-code-btn"></button></code></pre>
<p>输出结果是一个字典，其中包含受影响实体的统计数据。有关分区操作的详细信息，请参阅<a href="/docs/zh/manage-partitions.md">管理分区</a>。</p>
<h2 id="Upsert-entities" class="common-anchor-header">倒插实体<button data-href="#Upsert-entities" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><p>向上插入数据是更新和插入操作的组合。在 Milvus 中，upsert 操作执行数据级操作，根据实体的主键是否已存在于 Collections 中来插入或更新实体。具体来说</p>
<ul>
<li><p>如果实体的主键已经存在于 Collections 中，现有实体将被覆盖。</p></li>
<li><p>如果集合中不存在主键，则会插入一个新实体。</p></li>
</ul>
<div class="alert note">
<ul>
<li>向上插入操作不会更新主键。</li>
<li>如果计划在大规模数据摄取（如数百万向量）时使用<code translate="no">upsert</code> 操作符而不是<code translate="no">insert</code> ，请注意这会导致 Milvus 数据节点的内存消耗过高。</li>
</ul>
</div>
<div class="language-python">
<p>要向上插入实体，请使用 <a href="https://milvus.io/api-reference/pymilvus/v2.4.x/MilvusClient/Vector/upsert.md"><code translate="no">upsert()</code></a>方法。</p>
</div>
<div class="language-java">
<p>要插入实体，请使用 <a href="https://milvus.io/api-reference/java/v2.4.x/v2/Vector/insert.md"><code translate="no">upsert()</code></a>方法。</p>
</div>
<div class="language-javascript">
<p>要上载实体，请使用 <a href="https://milvus.io/api-reference/node/v2.4.x/Vector/upsert.md"><code translate="no">upsert()</code></a>方法。</p>
</div>
<div class="multipleCode">
   <a href="#python">Python </a> <a href="#java">Java</a> <a href="#javascript">Node.js</a></div>
<pre><code translate="no" class="language-python"><span class="hljs-comment"># 5. Upsert some data</span>
data=[
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">0</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.619954382375778</span>, <span class="hljs-number">0.4479436794798608</span>, -<span class="hljs-number">0.17493894838751745</span>, -<span class="hljs-number">0.4248030059917294</span>, -<span class="hljs-number">0.8648452746018911</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;black_9898&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">1</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.4762662251462588</span>, -<span class="hljs-number">0.6942502138717026</span>, -<span class="hljs-number">0.4490002642657902</span>, -<span class="hljs-number">0.628696575798281</span>, <span class="hljs-number">0.9660395877041965</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_7319&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">2</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.8864122635045097</span>, <span class="hljs-number">0.9260170474445351</span>, <span class="hljs-number">0.801326976181461</span>, <span class="hljs-number">0.6383943392381306</span>, <span class="hljs-number">0.7563037341572827</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;white_6465&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">3</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.14594326235891586</span>, -<span class="hljs-number">0.3775407299900644</span>, -<span class="hljs-number">0.3765479013078812</span>, <span class="hljs-number">0.20612075380355122</span>, <span class="hljs-number">0.4902678929632145</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_7580&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">4</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.4548498669607359</span>, -<span class="hljs-number">0.887610217681605</span>, <span class="hljs-number">0.5655081329910452</span>, <span class="hljs-number">0.19220509387904117</span>, <span class="hljs-number">0.016513983433433577</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_3314&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">5</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.11755001847051827</span>, -<span class="hljs-number">0.7295149788999611</span>, <span class="hljs-number">0.2608115847524266</span>, -<span class="hljs-number">0.1719167007897875</span>, <span class="hljs-number">0.7417611743754855</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;black_9955&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">6</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.9363032158314308</span>, <span class="hljs-number">0.030699901477745373</span>, <span class="hljs-number">0.8365910312319647</span>, <span class="hljs-number">0.7823840208444011</span>, <span class="hljs-number">0.2625222076909237</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;yellow_2461&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">7</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.0754823906014721</span>, -<span class="hljs-number">0.6390658668265143</span>, <span class="hljs-number">0.5610517334334937</span>, -<span class="hljs-number">0.8986261118798251</span>, <span class="hljs-number">0.9372056764266794</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;white_5015&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">8</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.3038434006935904</span>, <span class="hljs-number">0.1279149203380523</span>, <span class="hljs-number">0.503958664270957</span>, -<span class="hljs-number">0.2622661156746988</span>, <span class="hljs-number">0.7407627307791929</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;purple_6414&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">9</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.7125086947677588</span>, -<span class="hljs-number">0.8050968321012257</span>, -<span class="hljs-number">0.32608864121785786</span>, <span class="hljs-number">0.3255654958645424</span>, <span class="hljs-number">0.26227968923834233</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;brown_7231&quot;</span>}
]

res = client.upsert(
    collection_name=<span class="hljs-string">&#x27;quick_setup&#x27;</span>,
    data=data
)

<span class="hljs-built_in">print</span>(res)

<span class="hljs-comment"># Output</span>
<span class="hljs-comment">#</span>
<span class="hljs-comment"># {</span>
<span class="hljs-comment">#     &quot;upsert_count&quot;: 10</span>
<span class="hljs-comment"># }</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java"><span class="hljs-comment">// 5. Upsert some data</span>
data = <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 0L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>619954382375778f, <span class="hljs-number">0.</span>4479436794798608f, -<span class="hljs-number">0.</span>17493894838751745f, -<span class="hljs-number">0.</span>4248030059917294f, -<span class="hljs-number">0.</span>8648452746018911f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;black_9898&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 1L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>4762662251462588f, -<span class="hljs-number">0.</span>6942502138717026f, -<span class="hljs-number">0.</span>4490002642657902f, -<span class="hljs-number">0.</span>628696575798281f, <span class="hljs-number">0.</span>9660395877041965f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_7319&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 2L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>8864122635045097f, <span class="hljs-number">0.</span>9260170474445351f, <span class="hljs-number">0.</span>801326976181461f, <span class="hljs-number">0.</span>6383943392381306f, <span class="hljs-number">0.</span>7563037341572827f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;white_6465&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 3L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>14594326235891586f, -<span class="hljs-number">0.</span>3775407299900644f, -<span class="hljs-number">0.</span>3765479013078812f, <span class="hljs-number">0.</span>20612075380355122f, <span class="hljs-number">0.</span>4902678929632145f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_7580&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 4L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>4548498669607359f, -<span class="hljs-number">0.</span>887610217681605f, <span class="hljs-number">0.</span>5655081329910452f, <span class="hljs-number">0.</span>19220509387904117f, <span class="hljs-number">0.</span>016513983433433577f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_3314&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 5L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>11755001847051827f, -<span class="hljs-number">0.</span>7295149788999611f, <span class="hljs-number">0.</span>2608115847524266f, -<span class="hljs-number">0.</span>1719167007897875f, <span class="hljs-number">0.</span>7417611743754855f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;black_9955&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 6L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>9363032158314308f, <span class="hljs-number">0.</span>030699901477745373f, <span class="hljs-number">0.</span>8365910312319647f, <span class="hljs-number">0.</span>7823840208444011f, <span class="hljs-number">0.</span>2625222076909237f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;yellow_2461&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 7L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>0754823906014721f, -<span class="hljs-number">0.</span>6390658668265143f, <span class="hljs-number">0.</span>5610517334334937f, -<span class="hljs-number">0.</span>8986261118798251f, <span class="hljs-number">0.</span>9372056764266794f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;white_5015&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 8L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>3038434006935904f, <span class="hljs-number">0.</span>1279149203380523f, <span class="hljs-number">0.</span>503958664270957f, -<span class="hljs-number">0.</span>2622661156746988f, <span class="hljs-number">0.</span>7407627307791929f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;purple_6414&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 9L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>7125086947677588f, -<span class="hljs-number">0.</span>8050968321012257f, -<span class="hljs-number">0.</span>32608864121785786f, <span class="hljs-number">0.</span>3255654958645424f, <span class="hljs-number">0.</span>26227968923834233f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;brown_7231&quot;</span>))
);

<span class="hljs-title class_">UpsertReq</span> upsertReq = <span class="hljs-title class_">UpsertReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">data</span>(data)
    .<span class="hljs-title function_">build</span>();

<span class="hljs-title class_">UpsertResp</span> upsertResp = client.<span class="hljs-title function_">upsert</span>(upsertReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(upsertResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;upsertCnt&quot;: 10}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript">// <span class="hljs-number">5.</span> Upsert some data
data = [
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">0</span>, vector: [-<span class="hljs-number">0.619954382375778</span>, <span class="hljs-number">0.4479436794798608</span>, -<span class="hljs-number">0.17493894838751745</span>, -<span class="hljs-number">0.4248030059917294</span>, -<span class="hljs-number">0.8648452746018911</span>], color: <span class="hljs-string">&quot;black_9898&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">1</span>, vector: [<span class="hljs-number">0.4762662251462588</span>, -<span class="hljs-number">0.6942502138717026</span>, -<span class="hljs-number">0.4490002642657902</span>, -<span class="hljs-number">0.628696575798281</span>, <span class="hljs-number">0.9660395877041965</span>], color: <span class="hljs-string">&quot;red_7319&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">2</span>, vector: [-<span class="hljs-number">0.8864122635045097</span>, <span class="hljs-number">0.9260170474445351</span>, <span class="hljs-number">0.801326976181461</span>, <span class="hljs-number">0.6383943392381306</span>, <span class="hljs-number">0.7563037341572827</span>], color: <span class="hljs-string">&quot;white_6465&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">3</span>, vector: [<span class="hljs-number">0.14594326235891586</span>, -<span class="hljs-number">0.3775407299900644</span>, -<span class="hljs-number">0.3765479013078812</span>, <span class="hljs-number">0.20612075380355122</span>, <span class="hljs-number">0.4902678929632145</span>], color: <span class="hljs-string">&quot;orange_7580&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">4</span>, vector: [<span class="hljs-number">0.4548498669607359</span>, -<span class="hljs-number">0.887610217681605</span>, <span class="hljs-number">0.5655081329910452</span>, <span class="hljs-number">0.19220509387904117</span>, <span class="hljs-number">0.016513983433433577</span>], color: <span class="hljs-string">&quot;red_3314&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">5</span>, vector: [<span class="hljs-number">0.11755001847051827</span>, -<span class="hljs-number">0.7295149788999611</span>, <span class="hljs-number">0.2608115847524266</span>, -<span class="hljs-number">0.1719167007897875</span>, <span class="hljs-number">0.7417611743754855</span>], color: <span class="hljs-string">&quot;black_9955&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">6</span>, vector: [<span class="hljs-number">0.9363032158314308</span>, <span class="hljs-number">0.030699901477745373</span>, <span class="hljs-number">0.8365910312319647</span>, <span class="hljs-number">0.7823840208444011</span>, <span class="hljs-number">0.2625222076909237</span>], color: <span class="hljs-string">&quot;yellow_2461&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">7</span>, vector: [<span class="hljs-number">0.0754823906014721</span>, -<span class="hljs-number">0.6390658668265143</span>, <span class="hljs-number">0.5610517334334937</span>, -<span class="hljs-number">0.8986261118798251</span>, <span class="hljs-number">0.9372056764266794</span>], color: <span class="hljs-string">&quot;white_5015&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">8</span>, vector: [-<span class="hljs-number">0.3038434006935904</span>, <span class="hljs-number">0.1279149203380523</span>, <span class="hljs-number">0.503958664270957</span>, -<span class="hljs-number">0.2622661156746988</span>, <span class="hljs-number">0.7407627307791929</span>], color: <span class="hljs-string">&quot;purple_6414&quot;</span>},
    {<span class="hljs-built_in">id</span>: <span class="hljs-number">9</span>, vector: [-<span class="hljs-number">0.7125086947677588</span>, -<span class="hljs-number">0.8050968321012257</span>, -<span class="hljs-number">0.32608864121785786</span>, <span class="hljs-number">0.3255654958645424</span>, <span class="hljs-number">0.26227968923834233</span>], color: <span class="hljs-string">&quot;brown_7231&quot;</span>}
]

res = <span class="hljs-keyword">await</span> client.upsert({
    collection_name: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    data: data,
})

console.log(res.upsert_cnt)

// Output
// 
// <span class="hljs-number">10</span>
// 
<button class="copy-code-btn"></button></code></pre>
<h3 id="Upsert-data-in-partitions" class="common-anchor-header">向上插入分区中的数据</h3><p>要向上插入数据到特定分区，可在插入请求中指定分区名称，如下所示：</p>
<div class="multipleCode">
   <a href="#python">Python </a> <a href="#java">Java</a> <a href="#javascript">Node.js</a></div>
<pre><code translate="no" class="language-python"><span class="hljs-comment"># 6. Upsert data in partitions</span>
data=[
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">10</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.06998888224297328</span>, <span class="hljs-number">0.8582816610326578</span>, -<span class="hljs-number">0.9657938677934292</span>, <span class="hljs-number">0.6527905683627726</span>, -<span class="hljs-number">0.8668460657158576</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;black_3651&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">11</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.6060703043917468</span>, -<span class="hljs-number">0.3765080534566074</span>, -<span class="hljs-number">0.7710758854987239</span>, <span class="hljs-number">0.36993888322346136</span>, <span class="hljs-number">0.5507513364206531</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;grey_2049&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">12</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.9041813104515337</span>, -<span class="hljs-number">0.9610546012461163</span>, <span class="hljs-number">0.20033003106083358</span>, <span class="hljs-number">0.11842506351635174</span>, <span class="hljs-number">0.8327356724591011</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;blue_6168&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">13</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.3202914977909075</span>, -<span class="hljs-number">0.7279137773695252</span>, -<span class="hljs-number">0.04747830871620273</span>, <span class="hljs-number">0.8266053056909548</span>, <span class="hljs-number">0.8277957187455489</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;blue_1672&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">14</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.2975811497890859</span>, <span class="hljs-number">0.2946936202691086</span>, <span class="hljs-number">0.5399463833894609</span>, <span class="hljs-number">0.8385334966677529</span>, -<span class="hljs-number">0.4450543984655133</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;pink_1601&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">15</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.04697464305600074</span>, -<span class="hljs-number">0.08509022265734134</span>, <span class="hljs-number">0.9067184632552001</span>, -<span class="hljs-number">0.2281912685064822</span>, -<span class="hljs-number">0.9747503428652762</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;yellow_9925&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">16</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.9363075919673911</span>, -<span class="hljs-number">0.8153981031085669</span>, <span class="hljs-number">0.7943039120490902</span>, -<span class="hljs-number">0.2093886809842529</span>, <span class="hljs-number">0.0771191335807897</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;orange_9872&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">17</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.050451522820639916</span>, <span class="hljs-number">0.18931572752321935</span>, <span class="hljs-number">0.7522886192190488</span>, -<span class="hljs-number">0.9071793089474034</span>, <span class="hljs-number">0.6032647330692296</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;red_6450&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">18</span>, <span class="hljs-string">&quot;vector&quot;</span>: [-<span class="hljs-number">0.9181544231141592</span>, <span class="hljs-number">0.6700755998126806</span>, -<span class="hljs-number">0.014174674636136642</span>, <span class="hljs-number">0.6325780463623432</span>, -<span class="hljs-number">0.49662222164032976</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;purple_7392&quot;</span>},
    {<span class="hljs-string">&quot;id&quot;</span>: <span class="hljs-number">19</span>, <span class="hljs-string">&quot;vector&quot;</span>: [<span class="hljs-number">0.11426945899602536</span>, <span class="hljs-number">0.6089190684002581</span>, -<span class="hljs-number">0.5842735738352236</span>, <span class="hljs-number">0.057050610092692855</span>, -<span class="hljs-number">0.035163433018196244</span>], <span class="hljs-string">&quot;color&quot;</span>: <span class="hljs-string">&quot;pink_4996&quot;</span>}
]

res = client.upsert(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    data=data,
    partition_name=<span class="hljs-string">&quot;partitionA&quot;</span>
)

<span class="hljs-built_in">print</span>(res)

<span class="hljs-comment"># Output</span>
<span class="hljs-comment">#</span>
<span class="hljs-comment"># {</span>
<span class="hljs-comment">#     &quot;upsert_count&quot;: 10</span>
<span class="hljs-comment"># }</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java"><span class="hljs-keyword">import</span> io.<span class="hljs-property">milvus</span>.<span class="hljs-property">v2</span>.<span class="hljs-property">service</span>.<span class="hljs-property">vector</span>.<span class="hljs-property">request</span>.<span class="hljs-property">UpsertReq</span>;
<span class="hljs-keyword">import</span> io.<span class="hljs-property">milvus</span>.<span class="hljs-property">v2</span>.<span class="hljs-property">service</span>.<span class="hljs-property">vector</span>.<span class="hljs-property">response</span>.<span class="hljs-property">UpsertResp</span>;

<span class="hljs-comment">// 6. Upsert data in parition</span>

data = <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 10L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>06998888224297328f, <span class="hljs-number">0.</span>8582816610326578f, -<span class="hljs-number">0.</span>9657938677934292f, <span class="hljs-number">0.</span>6527905683627726f, -<span class="hljs-number">0.</span>8668460657158576f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;black_3651&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 11L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>6060703043917468f, -<span class="hljs-number">0.</span>3765080534566074f, -<span class="hljs-number">0.</span>7710758854987239f, <span class="hljs-number">0.</span>36993888322346136f, <span class="hljs-number">0.</span>5507513364206531f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;grey_2049&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 12L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>9041813104515337f, -<span class="hljs-number">0.</span>9610546012461163f, <span class="hljs-number">0.</span>20033003106083358f, <span class="hljs-number">0.</span>11842506351635174f, <span class="hljs-number">0.</span>8327356724591011f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;blue_6168&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 13L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>3202914977909075f, -<span class="hljs-number">0.</span>7279137773695252f, -<span class="hljs-number">0.</span>04747830871620273f, <span class="hljs-number">0.</span>8266053056909548f, <span class="hljs-number">0.</span>8277957187455489f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;blue_1672&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 14L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>2975811497890859f, <span class="hljs-number">0.</span>2946936202691086f, <span class="hljs-number">0.</span>5399463833894609f, <span class="hljs-number">0.</span>8385334966677529f, -<span class="hljs-number">0.</span>4450543984655133f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;pink_1601&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 15L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>04697464305600074f, -<span class="hljs-number">0.</span>08509022265734134f, <span class="hljs-number">0.</span>9067184632552001f, -<span class="hljs-number">0.</span>2281912685064822f, -<span class="hljs-number">0.</span>9747503428652762f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;yellow_9925&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 16L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>9363075919673911f, -<span class="hljs-number">0.</span>8153981031085669f, <span class="hljs-number">0.</span>7943039120490902f, -<span class="hljs-number">0.</span>2093886809842529f, <span class="hljs-number">0.</span>0771191335807897f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;orange_9872&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 17L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>050451522820639916f, <span class="hljs-number">0.</span>18931572752321935f, <span class="hljs-number">0.</span>7522886192190488f, -<span class="hljs-number">0.</span>9071793089474034f, <span class="hljs-number">0.</span>6032647330692296f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;red_6450&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 18L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(-<span class="hljs-number">0.</span>9181544231141592f, <span class="hljs-number">0.</span>6700755998126806f, -<span class="hljs-number">0.</span>014174674636136642f, <span class="hljs-number">0.</span>6325780463623432f, -<span class="hljs-number">0.</span>49662222164032976f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;purple_7392&quot;</span>)),
    <span class="hljs-keyword">new</span> <span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>(<span class="hljs-title class_">Map</span>.<span class="hljs-title function_">of</span>(<span class="hljs-string">&quot;id&quot;</span>, 19L, <span class="hljs-string">&quot;vector&quot;</span>, <span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(<span class="hljs-number">0.</span>11426945899602536f, <span class="hljs-number">0.</span>6089190684002581f, -<span class="hljs-number">0.</span>5842735738352236f, <span class="hljs-number">0.</span>057050610092692855f, -<span class="hljs-number">0.</span>035163433018196244f), <span class="hljs-string">&quot;color&quot;</span>, <span class="hljs-string">&quot;pink_4996&quot;</span>))
);

upsertReq = <span class="hljs-title class_">UpsertReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">data</span>(data)
    .<span class="hljs-title function_">partitionName</span>(<span class="hljs-string">&quot;partitionA&quot;</span>)
    .<span class="hljs-title function_">build</span>();

upsertResp = client.<span class="hljs-title function_">upsert</span>(upsertReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(upsertResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;upsertCnt&quot;: 10}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript"><span class="hljs-comment">// 6. Upsert data in partitions</span>
data = [
    {<span class="hljs-attr">id</span>: <span class="hljs-number">10</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.06998888224297328</span>, <span class="hljs-number">0.8582816610326578</span>, -<span class="hljs-number">0.9657938677934292</span>, <span class="hljs-number">0.6527905683627726</span>, -<span class="hljs-number">0.8668460657158576</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;black_3651&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">11</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.6060703043917468</span>, -<span class="hljs-number">0.3765080534566074</span>, -<span class="hljs-number">0.7710758854987239</span>, <span class="hljs-number">0.36993888322346136</span>, <span class="hljs-number">0.5507513364206531</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;grey_2049&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">12</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.9041813104515337</span>, -<span class="hljs-number">0.9610546012461163</span>, <span class="hljs-number">0.20033003106083358</span>, <span class="hljs-number">0.11842506351635174</span>, <span class="hljs-number">0.8327356724591011</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;blue_6168&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">13</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.3202914977909075</span>, -<span class="hljs-number">0.7279137773695252</span>, -<span class="hljs-number">0.04747830871620273</span>, <span class="hljs-number">0.8266053056909548</span>, <span class="hljs-number">0.8277957187455489</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;blue_1672&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">14</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.2975811497890859</span>, <span class="hljs-number">0.2946936202691086</span>, <span class="hljs-number">0.5399463833894609</span>, <span class="hljs-number">0.8385334966677529</span>, -<span class="hljs-number">0.4450543984655133</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;pink_1601&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">15</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.04697464305600074</span>, -<span class="hljs-number">0.08509022265734134</span>, <span class="hljs-number">0.9067184632552001</span>, -<span class="hljs-number">0.2281912685064822</span>, -<span class="hljs-number">0.9747503428652762</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;yellow_9925&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">16</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.9363075919673911</span>, -<span class="hljs-number">0.8153981031085669</span>, <span class="hljs-number">0.7943039120490902</span>, -<span class="hljs-number">0.2093886809842529</span>, <span class="hljs-number">0.0771191335807897</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;orange_9872&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">17</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.050451522820639916</span>, <span class="hljs-number">0.18931572752321935</span>, <span class="hljs-number">0.7522886192190488</span>, -<span class="hljs-number">0.9071793089474034</span>, <span class="hljs-number">0.6032647330692296</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;red_6450&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">18</span>, <span class="hljs-attr">vector</span>: [-<span class="hljs-number">0.9181544231141592</span>, <span class="hljs-number">0.6700755998126806</span>, -<span class="hljs-number">0.014174674636136642</span>, <span class="hljs-number">0.6325780463623432</span>, -<span class="hljs-number">0.49662222164032976</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;purple_7392&quot;</span>},
    {<span class="hljs-attr">id</span>: <span class="hljs-number">19</span>, <span class="hljs-attr">vector</span>: [<span class="hljs-number">0.11426945899602536</span>, <span class="hljs-number">0.6089190684002581</span>, -<span class="hljs-number">0.5842735738352236</span>, <span class="hljs-number">0.057050610092692855</span>, -<span class="hljs-number">0.035163433018196244</span>], <span class="hljs-attr">color</span>: <span class="hljs-string">&quot;pink_4996&quot;</span>}
]

res = <span class="hljs-keyword">await</span> client.<span class="hljs-title function_">upsert</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">data</span>: data,
    <span class="hljs-attr">partition_name</span>: <span class="hljs-string">&quot;partitionA&quot;</span>
})

<span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res.<span class="hljs-property">upsert_cnt</span>)

<span class="hljs-comment">// Output</span>
<span class="hljs-comment">// </span>
<span class="hljs-comment">// 10</span>
<span class="hljs-comment">// </span>
<button class="copy-code-btn"></button></code></pre>
<p>输出结果是一个字典，其中包含受影响实体的统计数据。有关分区操作的详细信息，请参阅<a href="/docs/zh/manage-partitions.md">管理分区</a>。</p>
<h2 id="Delete-entities" class="common-anchor-header">删除实体<button data-href="#Delete-entities" class="anchor-icon" translate="no">
      <svg translate="no"
        aria-hidden="true"
        focusable="false"
        height="20"
        version="1.1"
        viewBox="0 0 16 16"
        width="16"
      >
        <path
          fill="#0092E4"
          fill-rule="evenodd"
          d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"
        ></path>
      </svg>
    </button></h2><div class="language-python">
<p>如果不再需要某个实体，可以通过使用 <a href="https://milvus.io/api-reference/pymilvus/v2.4.x/MilvusClient/Vector/delete.md"><code translate="no">delete()</code></a>.</p>
</div>
<div class="language-java">
<p>如果不再需要某个实体，可以通过使用 <a href="https://milvus.io/api-reference/java/v2.4.x/v2/Vector/delete.md"><code translate="no">delete()</code></a>.</p>
</div>
<div class="language-javascript">
<p>如果不再需要某个实体，可以使用 . <a href="https://milvus.io/api-reference/node/v2.4.x/Vector/delete.md"><code translate="no">delete()</code></a>.</p>
</div>
<p>Milvus 提供两种方法让你确定要删除的实体。</p>
<ul>
<li><p><strong>通过过滤器删除实体。</strong></p>
   <div class='alert note'>
<p>使用过滤表达式删除实体时，请确保已加载集合。否则，Milvus 将返回错误。</p>
   </div>
<p><div class="multipleCode">
<a href="#python">Python </a><a href="#java">Java</a><a href="#javascript">Node.js</a></div></p>
<pre><code translate="no" class="language-python"><span class="hljs-comment"># 7. Delete entities</span>
res = client.delete(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-built_in">filter</span>=<span class="hljs-string">&quot;id in [4,5,6]&quot;</span>
)

<span class="hljs-built_in">print</span>(res)

<span class="hljs-comment"># Output</span>
<span class="hljs-comment">#</span>
<span class="hljs-comment"># {</span>
<span class="hljs-comment">#     &quot;delete_count&quot;: 3</span>
<span class="hljs-comment"># }</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java"><span class="hljs-keyword">import</span> io.<span class="hljs-property">milvus</span>.<span class="hljs-property">v2</span>.<span class="hljs-property">service</span>.<span class="hljs-property">vector</span>.<span class="hljs-property">request</span>.<span class="hljs-property">DeleteReq</span>;
<span class="hljs-keyword">import</span> io.<span class="hljs-property">milvus</span>.<span class="hljs-property">v2</span>.<span class="hljs-property">service</span>.<span class="hljs-property">vector</span>.<span class="hljs-property">response</span>.<span class="hljs-property">DeleteResp</span>;


<span class="hljs-comment">// 7. Delete entities</span>

<span class="hljs-title class_">DeleteReq</span> deleteReq = <span class="hljs-title class_">DeleteReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">filter</span>(<span class="hljs-string">&quot;id in [4, 5, 6]&quot;</span>)
    .<span class="hljs-title function_">build</span>();

<span class="hljs-title class_">DeleteResp</span> deleteResp = client.<span class="hljs-title function_">delete</span>(deleteReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(deleteResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;deleteCnt&quot;: 3}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript"><span class="hljs-comment">// 7. Delete entities</span>
res = <span class="hljs-keyword">await</span> client.<span class="hljs-title function_">delete</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">filter</span>: <span class="hljs-string">&quot;id in [4,5,6]&quot;</span>
})

<span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res.<span class="hljs-property">delete_cnt</span>)

<span class="hljs-comment">// Output</span>
<span class="hljs-comment">// </span>
<span class="hljs-comment">// 3</span>
<span class="hljs-comment">// </span>
<button class="copy-code-btn"></button></code></pre></li>
<li><p><strong>通过 ID 删除实体</strong></p>
<p>以下代码段演示了如何按 ID 从特定分区删除实体。如果不指定分区名称，该方法也同样有效。</p>
<p><div class="multipleCode">
<a href="#python">Python </a><a href="#java">Java</a><a href="#javascript">Node.js</a></div></p>
<pre><code translate="no" class="language-python">res = client.delete(
    collection_name=<span class="hljs-string">&quot;quick_setup&quot;</span>,
    ids=[<span class="hljs-number">18</span>, <span class="hljs-number">19</span>],
    partition_name=<span class="hljs-string">&quot;partitionA&quot;</span>
)

<span class="hljs-built_in">print</span>(res)

<span class="hljs-comment"># Output</span>
<span class="hljs-comment">#</span>
<span class="hljs-comment"># {</span>
<span class="hljs-comment">#     &quot;delete_count&quot;: 2</span>
<span class="hljs-comment"># }</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java">deleteReq = <span class="hljs-title class_">DeleteReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">ids</span>(<span class="hljs-title class_">Arrays</span>.<span class="hljs-title function_">asList</span>(18L, 19L))
    .<span class="hljs-title function_">partitionName</span>(<span class="hljs-string">&quot;partitionA&quot;</span>)
    .<span class="hljs-title function_">build</span>();

deleteResp = client.<span class="hljs-title function_">delete</span>(deleteReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(deleteResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;deleteCnt&quot;: 2}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript">res = <span class="hljs-keyword">await</span> client.<span class="hljs-title function_">delete</span>({
    <span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
    <span class="hljs-attr">ids</span>: [<span class="hljs-number">18</span>, <span class="hljs-number">19</span>],
    <span class="hljs-attr">partition_name</span>: <span class="hljs-string">&quot;partitionA&quot;</span>
})

<span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res.<span class="hljs-property">delete_cnt</span>)

<span class="hljs-comment">// Output</span>
<span class="hljs-comment">// </span>
<span class="hljs-comment">// 2</span>
<span class="hljs-comment">// </span>
<button class="copy-code-btn"></button></code></pre>
<p>有关如何使用过滤表达式的详细信息，请参阅<a href="/docs/zh/get-and-scalar-query.md">获取和标量查询</a>。</p></li>
<li><p><strong>按分区名称删除实体</strong></p>
<p>如果想删除特定分区中的实体，可以在<code translate="no">delete()</code> 方法中使用<code translate="no">partition_name</code> 参数指定分区名称。下面的示例删除了<code translate="no">partitionA</code> 中颜色以<code translate="no">blue</code> 开头的实体。</p>
<p><div class="multipleCode">
<a href="#python">Python </a><a href="#java">Java</a><a href="#javascript">Node.js</a></div></p>
<pre><code translate="no" class="language-python">res = client.delete(
collection_name=<span class="hljs-string">&#x27;quick_setup&#x27;</span>,
partition_name=<span class="hljs-string">&#x27;partitionA&#x27;</span>,
<span class="hljs-built_in">filter</span>=<span class="hljs-string">&#x27;color like &quot;blue%&quot;&#x27;</span>
)

<span class="hljs-built_in">print</span>(<span class="hljs-string">&quot;Entities deleted from partitionA: &quot;</span>, res[<span class="hljs-string">&#x27;delete_count&#x27;</span>])

<span class="hljs-comment"># Output:</span>
<span class="hljs-comment"># Entities deleted from partitionA:  3</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-java">deleteReq = <span class="hljs-title class_">DeleteReq</span>.<span class="hljs-title function_">builder</span>()
    .<span class="hljs-title function_">collectionName</span>(<span class="hljs-string">&quot;quick_setup&quot;</span>)
    .<span class="hljs-title function_">filter</span>(<span class="hljs-string">&#x27;color like &quot;blue%&quot;&#x27;</span>)
    .<span class="hljs-title function_">partitionName</span>(<span class="hljs-string">&quot;partitionA&quot;</span>)
    .<span class="hljs-title function_">build</span>();

deleteResp = client.<span class="hljs-title function_">delete</span>(deleteReq);

<span class="hljs-title class_">System</span>.<span class="hljs-property">out</span>.<span class="hljs-title function_">println</span>(<span class="hljs-title class_">JSON</span><span class="hljs-built_in">Object</span>.<span class="hljs-title function_">toJSON</span>(deleteResp));

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// {&quot;deleteCnt&quot;: 3}</span>
<button class="copy-code-btn"></button></code></pre>
<pre><code translate="no" class="language-javascript">res = <span class="hljs-keyword">await</span> client.<span class="hljs-title function_">delete</span>({
<span class="hljs-attr">collection_name</span>: <span class="hljs-string">&quot;quick_setup&quot;</span>,
<span class="hljs-attr">partition_name</span>: <span class="hljs-string">&quot;partitionA&quot;</span>,
<span class="hljs-attr">filter</span>: <span class="hljs-string">&#x27;color like &quot;blue%&quot;&#x27;</span>
})

<span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(<span class="hljs-string">&quot;Entities deleted from partitionA: &quot;</span> + res.<span class="hljs-property">delete_cnt</span>)

<span class="hljs-comment">// Output:</span>
<span class="hljs-comment">// Entities deleted from partitionA: 3</span>
<button class="copy-code-btn"></button></code></pre></li>
</ul>
