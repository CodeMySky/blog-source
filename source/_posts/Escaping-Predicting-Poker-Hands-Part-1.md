---
title: 'Escaping: Predicting Poker Hands, Part 1'
date: 2016-01-17 15:11:08
tags: [predicting-poker-hands]
---
Our hero Tom, who used to be a data scientist, went on an adventure, predicting poker hands. He wanted to share his experience here and he hopes it will be helpful for you to learn how to explore the big data world.

In this beginner's summary, Tom will first talk about some basic concepts of big data, including training sets, test sets. Tom then will talk about a random forest model and how to rate models in general. In the end, Tom will show why features are important to generate accurate results.
<!-- more -->

While Tom was traveling alone in the desert, he was caught by a mysterious tribe. The tribe really liked to play card games, so they proposed that, Tom could earn his freedom by winning a card game. Tom didn't know anything about their card games, but the tribe didn't tell him anything. So Tom observed them quietly and from their casual conversation, he understood some of their poker hands. Tom used his big data skills to predict the poker hands and escaped!

# Data Set
According to Tom's observation, on each card there is a wired mark and a number. Tom drew them down: <i class="fa fa-gear"></i> <i class="fa fa-tree"></i> <i class="fa fa-fire"></i> <i class="fa fa-flash"></i>. Tom named these marks 1, 2, 3, 4 for convenience. And there seemed to be 13 values of cards, ranging from 1 to 13. Tom wrote them down one line at a time, using two numbers represent one card, and the last number represent the type of poker hand, there are 10 types of poker hands possible.

An example of the data is:

    3, 5, 3, 6, 3, 9, 3, 7, 3, 8, 8
Here, `(3, 5)`, `(3, 6)`, `(3, 9)`, `(3, 7)`, `(3, 8)` represents five cards. And `8` represents one type of the poker hand.  All his records can be downloaded [here](/2016/01/08/Escaping-Predicting-Poker-Hands-Part-1/poker-hand-training-true.data.zip).

After he collected so many records, Tom decided to do some basic exploration. He decided to explore the distribution of the data. By observing data carefully, he was finally able to get some insight and unveil the mystery of the card game.

<div id="category-destribution" style="height:300px;"></div>
According to the distribution, we can see that some of the types are extremely rare, but they may be very valuable, just like Type 9

# Conclusion
Usually it's very hard to predict an open system, like the stock market, while it is quite easy to predict the behavior of a closed system. Unfortunately, most of the closed systems are already quite well studied by scientists. Nevertheless, today Tom is exploring a closed system, a game where the rules are explicitly defined. So Tom has luck now, his journey continues.

# Acknowledgement
The original data set is obtained from [UCI](http://archive.ics.uci.edu/ml/machine-learning-databases/poker/ "UCI Data Set").

<script src="/js/echart/echarts.js"></script>
<script type="text/javascript">
    require.config({
        paths: {
            echarts: '/js/echart'
        }
    });

    require(
        [
            'echarts',
            'echarts/chart/bar' // 使用柱状图就加载bar模块，按需加载
        ],
        function (ec) {
            var myChart = ec.init(document.getElementById('category-destribution'));

            var option = {
                title : {
                    text: 'Type Bar Chart',
                    subtext: ''
                },
                tooltip : {
                    show: true
                },
                toolbox: {
                    show : true,
                    feature : {
                        dataView : {show: true, readOnly: true},
                        saveAsImage : {show: true}
                    }
                },
                calculable : true,
                xAxis : [
                    {
                        type : 'category',
                        data : ['Type 0','Type 1','Type 2','Type 3','Type 4','Type 5','Type 6','Type 7','Type 8','Type 9']
                    }
                ],
                yAxis : [
                    {
                       type: 'value',
                    }
                ],
                series : [
                    {
                        name:'Count',
                        type:'bar',
                        data:[12493,10599,1206,513,93,54,36,6,5,5],
                    },
                ]
            };


            // 为echarts对象加载数据
            myChart.setOption(option);
            window.onresize = function(){
                myChart.resize();    
             };
        }
    );
</script>
