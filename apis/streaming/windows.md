---
title: "窗口操作"

sub-nav-id: 窗口
sub-nav-group: 流
sub-nav-pos: 3
---
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

* This will be replaced by the TOC
{:toc}

## 在keyed数据流上的窗口操作

Flink提供了多种在keyedStream上定义窗口的方法。所有元素都是基于key的，即每个窗口包含了相同的key的所有元素。

### 基本的窗口构造

Flink提供了通用的窗口机制，可以提供灵活性，而且为共同的用例提供了一些预定义的窗口机制。在定义你自己的窗口之前，我们先来看下预定义的窗口可以为你的用例提供什么。

<div class="codetabs" markdown="1">
<div data-lang="java" markdown="1">

<br />

<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td><strong>翻滚时间窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义一个5秒钟的“翻滚”窗口。这意味着元素以5秒钟的时间戳分组，而且每个元素只属于1个窗口。
	  时间的概念由TimeCharacteristic(see time)来指定。 (see <a href="{{ site.baseurl }}/apis/streaming/time.html">time</a>).
    {% highlight java %}
keyedStream.timeWindow(Time.seconds(5));
    {% endhighlight %}
          </p>
        </td>
      </tr>
      <tr>
          <td><strong>滑动时间窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
          <td>
            <p>
             定义一个5秒钟的窗口，每秒钟滑动一次。这意味着元素以5秒钟的时间戳分组，每个元素可以属于不止1个窗口（因为窗口最多重叠4秒）。
             时间的概念由TimeCharacteristic(see time)来指定。 (see <a href="{{ site.baseurl }}/apis/streaming/time.html">time</a>).
      {% highlight java %}
keyedStream.timeWindow(Time.seconds(5), Time.seconds(1));
      {% endhighlight %}
            </p>
          </td>
        </tr>
      <tr>
        <td><strong>翻滚数量窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义一个1000个元素的翻滚窗口。这意味着元素根据到达的（等同于processing time到达）1000个元素来分组，每个元素只属于1个窗口。
    {% highlight java %}
keyedStream.countWindow(1000);
    {% endhighlight %}
        </p>
        </td>
      </tr>
      <tr>
      <td><strong>滑动数量窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
      <td>
        <p>
          定义一个1000个元素的窗口，以每100个元素滑动触发。这意味着每1000个元素被分成1组，每个元素可以属于多于1个窗口（因为元素最多有900次重合）
  {% highlight java %}
keyedStream.countWindow(1000, 100)
  {% endhighlight %}
        </p>
      </td>
    </tr>
  </tbody>
</table>

</div>

<div data-lang="scala" markdown="1">

<br />

<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td><strong>翻滚时间窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义一个5秒钟的“翻滚”窗口。这意味着元素以5秒钟的时间戳分组，而且每个元素只属于1个窗口。
          时间的概念由TimeCharacteristic(see time)来指定。 (see <a href="{{ site.baseurl }}/apis/streaming/time.html">time</a>).
    {% highlight scala %}
keyedStream.timeWindow(Time.seconds(5))
    {% endhighlight %}
          </p>
        </td>
      </tr>
      <tr>
          <td><strong>滑动时间窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
          <td>
            <p>
             定义一个5秒钟的窗口，每秒钟滑动一次。这意味着元素以5秒钟的时间戳分组，每个元素可以属于不止1个窗口（因为窗口最多重叠4秒）。
             时间的概念由TimeCharacteristic(see time)来指定。 (see <a href="{{ site.baseurl }}/apis/streaming/time.html">time</a>).
      {% highlight scala %}
keyedStream.timeWindow(Time.seconds(5), Time.seconds(1))
      {% endhighlight %}
            </p>
          </td>
        </tr>
      <tr>
        <td><strong>翻滚数量窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义一个1000个元素的翻滚窗口。这意味着元素根据到达的（等同于processing time到达）1000个元素来分组，每个元素只属于1个窗口。
    {% highlight scala %}
keyedStream.countWindow(1000)
    {% endhighlight %}
        </p>
        </td>
      </tr>
      <tr>
      <td><strong>滑动数量窗口</strong><br>KeyedStream &rarr; WindowedStream</td>
      <td>
        <p>
          定义一个1000个元素的窗口，以每100个元素滑动触发。这意味着每1000个元素被分成1组，每个元素可以属于多于1个窗口（因为元素最多有900次重合）
  {% highlight scala %}
keyedStream.countWindow(1000, 100)
  {% endhighlight %}
        </p>
      </td>
    </tr>
  </tbody>
</table>

</div>
</div>

### 高级窗口的构造

一般的窗口定义机制会使用更冗长的语法来定义更强大的窗口。例如，下面是一个例子，定义了
一个每秒钟滑动一次，每次包含5秒钟的数据，但是只有当100个元素到达窗口时才被触发，且每次被执行完，会留下10个元素在窗口中。

<div class="codetabs" markdown="1">
<div data-lang="java" markdown="1">
{% highlight java %}
keyedStream
    .window(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1))
    .trigger(CountTrigger.of(100))
    .evictor(CountEvictor.of(10));
{% endhighlight %}
</div>

<div data-lang="scala" markdown="1">
{% highlight scala %}
keyedStream
    .window(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1))
    .trigger(CountTrigger.of(100))
    .evictor(CountEvictor.of(10))
{% endhighlight %}
</div>
</div>

构造一个定制的窗口通常需要指定：（1）一个窗口分配器（2）一个触发器（可选的）（3）一个驱逐器（可选的）

“窗口分配器”定义了进来的元素是如何被分配给一个窗口的。一个窗口就是一个元素的逻辑分组，有一个开始值、结束值，等同于一个开始时间和一个结束时间。带有时间戳的元素（根据上面描述的时间的概念，这些值属于窗口的一部分）。

例如，SlidingEventTimeWindows 分配器定义了一个每秒钟滑动一次的，窗口大小是5秒钟。假设时间以毫秒为单位从0开始，那么我们有了6个重合的窗口：[0,5000], [1000,6000], [2000,7000], [3000, 8000], [4000, 9000], and [5000, 10000]. 每个即将到来的元素根据时间戳都被分配给了相应的窗口。例如，时间戳是2000的元素将被分配到前3个窗口中。Flink将窗口分配器打包，覆盖了最通用的案例。你可以通过扩展WindowAssigner类来编写自己的程序。

<div class="codetabs" markdown="1">

<div data-lang="java" markdown="1">
<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td><strong>全局窗口分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
	    相同的key下，所有的即将到来的元素都会被分配到同一个窗口。全局窗口不会包含默认的触发器，因此如果触发器没有明确指定，那么窗口永远不会被触发。
          </p>
    {% highlight java %}
stream.window(GlobalWindows.create());
    {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td><strong>翻滚时间窗口分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
            即将到来的元素都会基于自己的时间戳被分配给一个特定大小的窗口（例如下面例子中的1秒钟）。窗口没有重复部分，即每个元素被精确的分到一个窗口中。翻滚时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
          </p>
      {% highlight java %}
stream.window(TumblingEventTimeWindows.of(Time.seconds(1)));
      {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td><strong>滑动时间窗口分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
            即将到来的元素都会基于自己的时间戳被分配给一个特定大小的窗口（例如下面例子中的5秒钟）。窗口根据提供的值来滑动（例如例子中的1秒钟），因此窗口会有重合。滑动时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
          </p>
    {% highlight java %}
stream.window(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1)));
    {% endhighlight %}
        </td>
      </tr>
      <tr>
          <td><strong>翻滚系统时间分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
          <td>
            <p>
              即将到来的元素根据当前的系统时间分配给一个特定大小的窗口（下面的1秒钟）。窗口没有重复部分，即每个元素被精确的分到一个窗口中。翻滚系统时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
            </p>
      {% highlight java %}
stream.window(TumblingProcessingTimeWindows.of(Time.seconds(1)));
      {% endhighlight %}
          </td>
        </tr>
      <tr>
        <td><strong>滑动系统时间分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
            即将到来的元素都会基于系统时间被分配给一个特定大小的窗口（例如下面例子中的5秒钟）。窗口根据提供的值来滑动（例如例子中的1秒钟），因此窗口会有重合。滑动时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
          </p>
    {% highlight java %}
stream.window(SlidingProcessingTimeWindows.of(Time.seconds(5), Time.seconds(1)));
    {% endhighlight %}
        </td>
      </tr>
  </tbody>
</table>
</div>

<div data-lang="scala" markdown="1">
<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td><strong>全局窗口分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
            相同的key下，所有的即将到来的元素都会被分配到同一个窗口。全局窗口不会包含默认的触发器，因此如果触发器没有明确指定，那么窗口永远不会被触发。
          </p>
    {% highlight scala %}
stream.window(GlobalWindows.create)
    {% endhighlight %}
        </td>
      </tr>
      <tr>
          <td><strong>翻滚时间窗口分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
          <td>
            <p>
             即将到来的元素都会基于自己的时间戳被分配给一个特定大小的窗口（例如下面例子中的1秒钟）。窗口没有重复部分，即每个元素被精确的分到一个窗口中。翻滚时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
            </p>
      {% highlight scala %}
stream.window(TumblingEventTimeWindows.of(Time.seconds(1)))
      {% endhighlight %}
          </td>
        </tr>
      <tr>
        <td><strong>滑动时间窗口分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
            即将到来的元素都会基于自己的时间戳被分配给一个特定大小的窗口（例如下面例子中的5秒钟）。窗口根据提供的值来滑动（例如例子中的1秒钟），因此窗口会有重合。滑动时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
          </p>
    {% highlight scala %}
stream.window(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1)))
    {% endhighlight %}
        </td>
      </tr>
      <tr>
          <td><strong>翻滚系统时间分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
          <td>
            <p>
              即将到来的元素根据当前的系统时间分配给一个特定大小的窗口（下面的1秒钟）。窗口没有重复部分，即每个元素被精确的分到一个窗口中。翻滚系统时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。

            </p>
      {% highlight scala %}
stream.window(TumblingProcessingTimeWindows.of(Time.seconds(1)))
      {% endhighlight %}
          </td>
        </tr>
      <tr>
        <td><strong>滑动系统时间分配器</strong><br>KeyedStream &rarr; WindowedStream</td>
        <td>
          <p>
            即将到来的元素都会基于系统时间被分配给一个特定大小的窗口（例如下面例子中的5秒钟）。窗口根据提供的值来滑动（例如例子中的1秒钟），因此窗口会有重合。滑动时间窗口分配器有一个默认的触发器，当水位线的值高于它最后接收到的值的时候，窗口会被触发。
          </p>
    {% highlight scala %}
stream.window(SlidingProcessingTimeWindows.of(Time.seconds(5), Time.seconds(1)))
    {% endhighlight %}
        </td>
      </tr>
  </tbody>
</table>
</div>

</div>

触发器为每个窗口指出了窗口语句之后的函数（例如sum、count）何时被计算（触发）。每个窗口可以键入一个默认的触发器（在定义WindowAssigner时作为其一部分来指出）。Flink打包了一些触发器，适用于窗口的默认触发器不能满足应用程序时使用。你可以继承Trigger接口实现自己的触发器。 注意，指定的触发器将会覆盖窗口分配器默认的触发器。

<div class="codetabs" markdown="1">

<div data-lang="java" markdown="1">
<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td><strong>系统时间触发器</strong></td>
    <td>
      <p>
        当前的系统时间超过了它的最后的值时，窗口会被触发。在触发窗口中的元素之后会被抛弃。
      </p>
{% highlight java %}
windowedStream.trigger(ProcessingTimeTrigger.create());
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>水位线触发器</strong></td>
    <td>
      <p>
        当水位线的值超过了窗口接收到的最后的值时，窗口会被触发。在触发窗口中的元素之后会被抛弃。
      </p>
{% highlight java %}
windowedStream.trigger(EventTimeTrigger.create());
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>连续系统时间触发器</strong></td>
    <td>
      <p>
        窗口被周期性的触发（例子中的5秒钟）。实际上窗口仅当当前时间超过它的最后的值时才被触发。在触发窗口中的元素会被保留。
      </p>
{% highlight java %}
windowedStream.trigger(ContinuousProcessingTimeTrigger.of(Time.seconds(5)));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>连续水位线触发器</strong></td>
    <td>
      <p>
        窗口被周期性的触发（例子中的5秒钟）。实际上窗口仅当水位线的时间超过它的最后的值时才被触发。在触发窗口中的元素会被保留。
      </p>
{% highlight java %}
windowedStream.trigger(ContinuousEventTimeTrigger.of(Time.seconds(5)));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>数量触发器</strong></td>
    <td>
      <p>
        当多于一定数量时（例子中1000个）窗口被触发。在触发窗口中的元素会被保留。
      </p>
{% highlight java %}
windowedStream.trigger(CountTrigger.of(1000));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>清除触发器</strong></td>
    <td>
      <p>
        把触发器当做参数强迫触发窗口中的元素在触发后被清除（抛弃）。
      </p>
{% highlight java %}
windowedStream.trigger(PurgingTrigger.of(CountTrigger.of(1000)));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>增量触发器</strong></td>
    <td>
      <p>
        窗口被周期性的触发（例子中的5000毫秒）。实际上窗口被触发的条件是根据“增量函数”，当最后添加的元素超过了第一个添加的元素时会被触发。
      </p>
{% highlight java %}
windowedStream.trigger(new DeltaTrigger.of(5000.0, new DeltaFunction<Double>() {
    @Override
    public double getDelta (Double old, Double new) {
        return (new - old > 0.01);
    }
}));
{% endhighlight %}
    </td>
  </tr>
 </tbody>
</table>
</div>


<div data-lang="scala" markdown="1">
<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td><strong>系统时间触发器</strong></td>
    <td>
      <p>
        当当前的系统时间超过了它的最后的值时，窗口会被触发。在触发窗口中的元素之后会被抛弃。
      </p>
{% highlight scala %}
windowedStream.trigger(ProcessingTimeTrigger.create);
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>水位线触发器</strong></td>
    <td>
      <p>
        当水位线的值超过了窗口接收到的最后的值时，窗口会被触发。在触发窗口中的元素之后会被抛弃。
      </p>
{% highlight scala %}
windowedStream.trigger(EventTimeTrigger.create);
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>连续系统时间触发器</strong></td>
    <td>
      <p>
        窗口被周期性的触发（例子中的5秒钟）。实际上窗口仅当当前时间超过它的最后的值时才被触发。在触发窗口中的元素会被保留。
      </p>
{% highlight scala %}
windowedStream.trigger(ContinuousProcessingTimeTrigger.of(Time.seconds(5)));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>连续水位线触发器</strong></td>
    <td>
      <p>
        窗口被周期性的触发（例子中的5秒钟）。实际上窗口仅当水位线的时间超过它的最后的值时才被触发。在触发窗口中的元素会被保留。
      </p>
{% highlight scala %}
windowedStream.trigger(ContinuousEventTimeTrigger.of(Time.seconds(5)));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>数量触发器</strong></td>
    <td>
      <p>
        当多于一定数量时（例子中1000个）窗口被触发。在触发窗口中的元素会被保留。
      </p>
{% highlight scala %}
windowedStream.trigger(CountTrigger.of(1000));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>清除触发器</strong></td>
    <td>
      <p>
        把触发器当做参数强迫触发窗口中的元素在触发后被清除（抛弃）。
      </p>
{% highlight scala %}
windowedStream.trigger(PurgingTrigger.of(CountTrigger.of(1000)));
{% endhighlight %}
    </td>
  </tr>
  <tr>
    <td><strong>增量触发器</strong></td>
    <td>
      <p>
        窗口被周期性的触发（例子中的5000毫秒）。实际上窗口被触发的条件是根据“增量函数”，当最后添加的元素超过了第一个添加的元素时会被触发。
      </p>
{% highlight scala %}
windowedStream.trigger(DeltaTrigger.of(5000.0, { (old,new) => new - old > 0.01 }))
{% endhighlight %}
    </td>
  </tr>
 </tbody>
</table>
</div>

</div>

在触发器被触发之后，在函数被应用之前，一个可选的Evictor（驱逐器）会从开始的窗口中移除一部分元素。Flink打包了一些驱逐器，你可以通过集成Evictor接口来编写程序。

<div class="codetabs" markdown="1">

<div data-lang="java" markdown="1">
<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
  <tr>
      <td><strong>时间驱逐器</strong></td>
      <td>
        <p>
         驱逐从窗口开始时的元素，以便从窗口最后的元素-1秒到窗口最后的元素的时间范围内被保留（窗口保留的大小是1秒）
        </p>
  {% highlight java %}
triggeredStream.evictor(TimeEvictor.of(Time.seconds(1)));
  {% endhighlight %}
      </td>
    </tr>
   <tr>
       <td><strong>数量驱逐器</strong></td>
       <td>
         <p>
          窗口从后往前推，保留1000个元素。
         </p>
   {% highlight java %}
triggeredStream.evictor(CountEvictor.of(1000));
   {% endhighlight %}
       </td>
     </tr>
    <tr>
        <td><strong>增量驱逐器</strong></td>
        <td>
          <p>
            从窗口起始部分开始，驱逐元素，直到有元素低于最后一个元素的值时。（通过一个临界值和一个增量函数）
          </p>
    {% highlight java %}
triggeredStream.evictor(DeltaEvictor.of(5000, new DeltaFunction<Double>() {
  public double (Double oldValue, Double newValue) {
      return newValue - oldValue;
  }
}));
    {% endhighlight %}
        </td>
      </tr>
 </tbody>
</table>
</div>

<div data-lang="scala" markdown="1">
<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
  <tr>
      <td><strong>时间驱逐器</strong></td>
      <td>
        <p>
         驱逐从窗口开始时的元素，以便从窗口最后的元素-1秒到窗口最后的元素的时间范围内被保留（窗口保留的大小是1秒）
        </p>
  {% highlight scala %}
triggeredStream.evictor(TimeEvictor.of(Time.seconds(1)));
  {% endhighlight %}
      </td>
    </tr>
   <tr>
       <td><strong>数量驱逐器</strong></td>
       <td>
         <p>
          窗口从后往前推，保留1000个元素。
         </p>
   {% highlight scala %}
triggeredStream.evictor(CountEvictor.of(1000));
   {% endhighlight %}
       </td>
     </tr>
    <tr>
        <td><strong>增量驱逐器</strong></td>
        <td>
          <p>
            从窗口起始部分开始，驱逐元素，直到有元素低于最后一个元素的值时。（通过一个临界值和一个增量函数）
          </p>
    {% highlight scala %}
windowedStream.evictor(DeltaEvictor.of(5000.0, { (old,new) => new - old > 0.01 }))
    {% endhighlight %}
        </td>
      </tr>
 </tbody>
</table>
</div>

</div>

### 构建窗口的方法

窗口分配器、触发器、驱逐器的机制是非常强大的，而且它允许你定义许多不同类型的窗口。事实上，Flink的基本窗口构造就是通常机制上的语法糖。下面是使用通用机制的窗口的一般构造方法。

<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 35%">窗口类型</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td>
	  <strong>基于数量的翻滚窗口</strong><br>
    {% highlight java %}
stream.countWindow(1000)
    {% endhighlight %}
	</td>
        <td>
    {% highlight java %}
stream.window(GlobalWindows.create())
  .trigger(PurgingTrigger.of(CountTrigger.of(size)))
    {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td>
	  <strong>基于数量的滑动窗口</strong><br>
    {% highlight java %}
stream.countWindow(1000, 100)
    {% endhighlight %}
	</td>
        <td>
    {% highlight java %}
stream.window(GlobalWindows.create())
  .evictor(CountEvictor.of(1000))
  .trigger(CountTrigger.of(100))
    {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td>
	  <strong>基于事件时间的翻滚窗口</strong><br>
    {% highlight java %}
stream.timeWindow(Time.seconds(5))
    {% endhighlight %}
	</td>
        <td>
    {% highlight java %}
stream.window(TumblingEventTimeWindows.of((Time.seconds(5)))
  .trigger(EventTimeTrigger.create())
    {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td>
	  <strong>基于事件时间的滑动窗口</strong><br>
    {% highlight java %}
stream.timeWindow(Time.seconds(5), Time.seconds(1))
    {% endhighlight %}
	</td>
        <td>
    {% highlight java %}
stream.window(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1)))
  .trigger(EventTimeTrigger.create())
    {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td>
	  <strong>基于系统时间的翻滚窗口</strong><br>
    {% highlight java %}
stream.timeWindow(Time.seconds(5))
    {% endhighlight %}
	</td>
        <td>
    {% highlight java %}
stream.window(TumblingProcessingTimeWindows.of((Time.seconds(5)))
  .trigger(ProcessingTimeTrigger.create())
    {% endhighlight %}
        </td>
      </tr>
      <tr>
        <td>
	  <strong>基于系统时间的的滑动窗口</strong><br>
    {% highlight java %}
stream.timeWindow(Time.seconds(5), Time.seconds(1))
    {% endhighlight %}
	</td>
        <td>
    {% highlight java %}
stream.window(SlidingProcessingTimeWindows.of(Time.seconds(5), Time.seconds(1)))
  .trigger(ProcessingTimeTrigger.create())
    {% endhighlight %}
        </td>
      </tr>
  </tbody>
</table>


## 在非keyed数据流上的窗口操作

你同样可以通过使用WindowALL转换操作在普通的（非keyed）数据流上定义窗口。这些窗口化了的数据流与keyed窗口中的数据流有相同的处理能力，但却是在一个单独的任务（因此是单节点的计算）中被计算。定义触发器、驱逐器的语法完全一样：

<div class="codetabs" markdown="1">
<div data-lang="java" markdown="1">
{% highlight java %}
nonKeyedStream
    .windowAll(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1))
    .trigger(CountTrigger.of(100))
    .evictor(CountEvictor.of(10));
{% endhighlight %}
</div>

<div data-lang="scala" markdown="1">
{% highlight scala %}
nonKeyedStream
    .windowAll(SlidingEventTimeWindows.of(Time.seconds(5), Time.seconds(1))
    .trigger(CountTrigger.of(100))
    .evictor(CountEvictor.of(10))
{% endhighlight %}
</div>
</div>

非keyed流的窗口定义同样在基本的窗口中可用：

<div class="codetabs" markdown="1">
<div data-lang="java" markdown="1">

<br />

<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td><strong>Window all的翻滚时间</strong><br>DataStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义了一个5秒钟的翻滚窗口。这意味着元素根据时间戳，以5秒钟的间隔被分组，每个元素精确的属于一个窗口。
          时间的概念在StreamExecutionEnvironment中指定。
    {% highlight java %}
nonKeyedStream.timeWindowAll(Time.seconds(5));
    {% endhighlight %}
          </p>
        </td>
      </tr>
      <tr>
          <td><strong>Window all的滑动时间</strong><br>DataStream &rarr; WindowedStream</td>
          <td>
            <p>
             定义了一个5秒钟的窗口，每秒钟滑动一次。这意味着元素根据时间戳，以5秒钟的间隔被分组，元素可以属于多个窗口（因为窗口最少重合4秒）。时间的概念在StreamExecutionEnvironment中指定。
      {% highlight java %}
nonKeyedStream.timeWindowAll(Time.seconds(5), Time.seconds(1));
      {% endhighlight %}
            </p>
          </td>
        </tr>
      <tr>
        <td><strong>Window all的翻滚数量</strong><br>DataStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义一个1000个元素的翻滚窗口。这意味着窗口以1000个元素分组，每个元素属于一个窗口。
    {% highlight java %}
nonKeyedStream.countWindowAll(1000)
    {% endhighlight %}
        </p>
        </td>
      </tr>
      <tr>
      <td><strong>Window all的滑动数量</strong><br>DataStream &rarr; WindowedStream</td>
      <td>
        <p>
          定义一个1000个元素的窗口，每100个元素滑动一次。这意味着以1000个元素分组，每个元素可以属于多个窗口（因为至少900个元素重合）。
  {% highlight java %}
nonKeyedStream.countWindowAll(1000, 100)
  {% endhighlight %}
        </p>
      </td>
    </tr>
  </tbody>
</table>

</div>

<div data-lang="scala" markdown="1">

<br />

<table class="table table-bordered">
  <thead>
    <tr>
      <th class="text-left" style="width: 25%">转换操作</th>
      <th class="text-center">描述</th>
    </tr>
  </thead>
  <tbody>
      <tr>
        <td><strong>Window all的翻滚时间</strong><br>DataStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义了一个5秒钟的翻滚窗口。这意味着元素根据时间戳，以5秒钟的间隔被分组，每个元素精确的属于一个窗口。
          时间的概念在StreamExecutionEnvironment中指定。
    {% highlight scala %}
nonKeyedStream.timeWindowAll(Time.seconds(5));
    {% endhighlight %}
          </p>
        </td>
      </tr>
      <tr>
          <td><strong>Window all的滑动时间</strong><br>DataStream &rarr; WindowedStream</td>
          <td>
            <p>
             定义了一个5秒钟的窗口，每秒钟滑动一次。这意味着元素根据时间戳，以5秒钟的间隔被分组，元素可以属于多个窗口（因为窗口最少重合4秒）。
             时间的概念在StreamExecutionEnvironment中指定。
      {% highlight scala %}
nonKeyedStream.timeWindowAll(Time.seconds(5), Time.seconds(1));
      {% endhighlight %}
            </p>
          </td>
        </tr>
      <tr>
        <td><strong>Window all的翻滚数量</strong><br>DataStream &rarr; WindowedStream</td>
        <td>
          <p>
          定义一个1000个元素的翻滚窗口。这意味着窗口以1000个元素分组，每个元素属于一个窗口。
    {% highlight scala %}
nonKeyedStream.countWindowAll(1000)
    {% endhighlight %}
        </p>
        </td>
      </tr>
      <tr>
      <td><strong>Window all的滑动数量</strong><br>DataStream &rarr; WindowedStream</td>
      <td>
        <p>
          定义一个1000个元素的窗口，每100个元素滑动一次。这意味着以1000个元素分组，每个元素可以属于多个窗口（因为至少900个元素重合）。
  {% highlight scala %}
nonKeyedStream.countWindowAll(1000, 100)
  {% endhighlight %}
        </p>
      </td>
    </tr>
  </tbody>
</table>

</div>
</div>
