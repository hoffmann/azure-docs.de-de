---
title: MapReduce mit Hadoop in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie MapReduce-Aufträge in Hadoop in HDInsight-Clustern ausgeführt werden. Sie führen einen einfachen Vorgang zum Zählen von Wörtern aus, der als Java MapReduce-Auftrag implementiert wird."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: b72443c0ca60196535ac093a6ac03df456f776ea
ms.openlocfilehash: 068cd703d0d06206b3caa72e765dbe51b819ff17


---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Verwenden von MapReduce mit Hadoop in HDInsight

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie MapReduce-Aufträge in Hadoop in HDInsight-Clustern ausgeführt werden. Es wird ein einfacher Vorgang zum Zählen von Wörtern als Java MapReduce-Auftrag implementiert.

## <a name="a-idwhatisawhat-is-mapreduce"></a><a id="whatis"></a>Was ist MapReduce?

Hadoop MapReduce ist ein Softwareframework zum Schreiben von Aufträgen, die sehr große Datenmengen verarbeiten. Die Eingabedaten werden in unabhängige Segmente unterteilt, die anschließend von den Knoten im Cluster parallel verarbeitet werden. Ein MapReduce-Auftrag besteht aus zwei Funktionen:

* **Mapper**: Verarbeitet Eingabedaten, analysiert sie (in der Regel mit Filter- und Sortiervorgängen) und gibt Tupel (Schlüssel-Wert-Paare) aus.

* **Reducer**: Verarbeitet Tupel, die vom Mapper ausgegeben wurden, und führt einen Zusammenfassungsvorgang aus, der kleinere, kombinierte Ergebnisse aus den Mapper-Daten erstellt.

Ein Beispiel für einen grundlegenden MapReduce-Auftrag zum Zählen von Wörtern wird im folgenden Diagramm veranschaulicht:

![HDI.Wortzähldiagramm][image-hdi-wordcountdiagram]

Dieser Auftrag gibt die Anzahl der Vorkommen jedes Worts im analysierten Text aus.

* Der Mapper verwendet die einzelnen Zeilen des Eingabetexts als Eingabe und unterteilt diese in Wörter. Er gibt bei jedem Vorkommen eines Worts ein Schlüssel-Wert-Paar des Worts gefolgt von 1 aus. Die Ausgabe wird sortiert, bevor sie an den Reducer gesendet wird.
* Der Reducer addiert die einzelnen Zählwerte jedes Worts und gibt ein einziges Schlüssel-Wert-Paar aus, das aus dem Wort gefolgt von der Summe seiner Vorkommen besteht.

MapReduce kann in einer Vielzahl von Sprachen implementiert werden. Java ist die am häufigsten verwendete Implementierung und wird in diesem Dokument zu Demonstrationszwecken verwendet.

### <a name="hadoop-streaming"></a>Hadoop-Datenströme

Sprachen oder Frameworks auf der Grundlage von Java und der Java Virtual Machine (z. B. Scalding oder Cascading) können direkt als MapReduce-Auftrag ausgeführt werden, ähnlich einer Java-Anwendung. Andere, wie z. B. C# oder Python bzw. eigenständige ausführbare Dateien, müssen Hadoop-Datenströme verwenden.

Hadoop-Datenströme kommunizieren über STDIN und STDOUT mit dem Mapper und dem Reducer. Der Mapper und der Reducer lesen Daten jeweils zeilenweise aus STDIN und schreiben die Ausgabe in STDOUT. Jede vom Mapper und vom Reducer gelesene oder ausgegebene Zeile muss als Schlüssel-Wert-Paar mit Tabstopptrennzeichen formatiert sein:

    [key]/t[value]

Weitere Informationen finden Sie unter [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html)(in englischer Sprache).

Beispiele für die Verwendung von Hadoop-Datenströmen mit HDInsight finden Sie hier:

* [Entwickeln von Python-MapReduce-Aufträgen](hdinsight-hadoop-streaming-python.md)

## <a name="a-iddataaabout-the-sample-data"></a><a id="data"></a>Infos zu Beispiel-Daten

In diesem Beispiel werden die Notizbücher von Leonardo Da Vinci als Beispieldaten verwendet, die in Form eines Textdokuments in Ihrem HDInsight-Cluster bereitgestellt werden.

Die Beispieldaten werden in einem Azure Blobspeicher gespeichert, den HDInsight als Standard-Dateisystem für Hadoop-Cluster verwendet. HDInsight kann mithilfe des Präfixes **wasb** auf Dateien zugreifen, die im Blobspeicher gespeichert sind. Für die Datei "sample.log" gilt z. B. die folgende Syntax:

    wasbs:///example/data/gutenberg/davinci.txt

Da Azure-Blobspeicher der Standardspeicher für HDInsight ist, können Sie auch über **/example/data/gutenberg/davinci.txt**auf die Datei zugreifen.

> [!NOTE]
> In der vorherigen Syntax wird **wasbs:///** für den Zugriff auf Dateien verwendet, die im Standardspeichercontainer für Ihren HDInsight-Cluster gespeichert werden. Wenn Sie beim Bereitstellen Ihres Clusters weitere Speicherkonten angegeben haben und auf die unter diesen Konten gespeicherten Dateien zugreifen möchten, können Sie hierfür den Containernamen und die Speicherkontoadresse angeben. Beispiel: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**


## <a name="a-idjobaabout-the-example-mapreduce"></a><a id="job"></a>Informationen zum Beispiel-MapReduce

Der MapReduce-Auftrag, der in diesem Beispiel verwendet wird, befindet sich unter **wasbs://example/jars/hadoop-mapreduce-examples.jar**, und er wird mit dem HDInsight-Cluster bereitgestellt. Er enthält ein Wortzählungsbeispiel, das Sie auf **davinci.txt** anwenden.

> [!NOTE]
> Bei HDInsight 2.1-Clustern lautet der Dateispeicherort **wasbs:///example/jars/hadoop-examples.jar**.

Zu Referenzzwecken ist im Folgenden der Java-Code für den MapReduce-Auftrag zum Zählen von Wörtern aufgeführt:

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Anweisungen zum Schreiben eines eigenen MapReduce-Auftrags finden Sie unter [Entwickeln von Java MapReduce-Programmen für HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

## <a name="a-idrunarun-the-mapreduce"></a><a id="run"></a>Ausführen von MapReduce

HDInsight kann HiveQL-Aufträge mithilfe verschiedener Methoden ausführen. Die folgende Tabelle hilft Ihnen bei der Entscheidung, welche Methode für Sie geeignet ist. Folgen Sie anschließend dem Link für eine exemplarische Vorgehensweise.

| **Option** | **Zweck** | ...mit diesem **Clusterbetriebssystem** | ...von diesem **Clientbetriebssystem** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Verwenden des Hadoop-Befehls über **SSH** |Linux |Linux, Unix, Mac OS X oder Windows |
| [Curl](hdinsight-hadoop-use-mapreduce-curl.md) |Remoteübermittlung des Auftrags mit **REST** |Linux oder Windows |Linux, Unix, Mac OS X oder Windows |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Remoteübermittlung des Auftrags mit **Windows PowerShell** |Linux oder Windows |Windows |
| [Remotedesktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) |Verwenden des Hadoop-Befehls über **Remotedesktop** |Windows |Windows |

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

MapReduce bietet zwar leistungsstarke Diagnosemöglichkeiten, ist jedoch eventuell etwas kompliziert in der Anwendung. Es gibt mehrere Java-basierte Frameworks, mit denen es einfacher ist, MapReduce-Anwendungen zu definieren, sowie Technologien wie Pig und Hive, die einfachere Methoden zur Arbeit mit Daten in HDInsight zur Verfügung stellen. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Entwickeln von Java MapReduce-Programmen für HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
* [Entwickeln von Python-Streamingprogrammen für HDInsight](hdinsight-hadoop-streaming-python.md)
* [Entwickeln von Scalding-MapReduce-Aufträgen mit Apache Hadoop in HDInsight](hdinsight-hadoop-mapreduce-scalding.md)
* [Verwenden von Hive mit HDInsight][hdinsight-use-hive]
* [Verwenden von Pig mit HDInsight][hdinsight-use-pig]
* [Ausführen der HDInsight-Beispiele][hdinsight-samples]

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif



<!--HONumber=Nov16_HO3-->


