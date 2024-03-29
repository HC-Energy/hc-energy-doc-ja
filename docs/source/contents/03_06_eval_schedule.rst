.. include:: definition.txt

.. raw:: latex

    \clearpage

========================================================================================================================
スケジュール
========================================================================================================================

------------------------------------------------------------------------------------------------------------------------
1 はじめに
------------------------------------------------------------------------------------------------------------------------

本節では、スケジュールの指定の仕方について定義する。

スケジュールは室ごとに設定される。

スケジュールの種類は、居住人数に依存しないスケジュール（例えば非居室など）と居住人数に依存するスケジュールがある。

居住人数に依存するスケジュールを取得する場合、居住人数の指定の仕方は次の2種類ある。
この指定は、住宅全体で統一した1つの方法を決める必要がある。（室ごとに指定の仕方を変えることはできない。）
    
    a. 1人・2人・3人・4人の中から居住人数を直接指定する方法
    b. 床面積から仮想的な居住人数を計算する方法

「1人・2人・3人・4人の中から居住人数を直接指定する方法」を採用する場合、本節の計算は次の値を必要とする。

    1. 居住人数
    2. 室 |i| の床面積 :math:`A_{f,i}`, |m2|
    3. 室 |i| の使用するスケジュールの名称
    4. 1日あたりのステップ数, :math:`n_{step,day}`

注：照明発熱スケジュールが単位面積あたりで定義されるため、居住人数を直接指定する方法であっても床面積の入力を必要とする。

「床面積から仮想的な居住人数を計算する方法」を採用する場合、本節の計算は次の値を必要とする。

    1. 室 |i| の床面積 :math:`A_{f,i}`, |m2|
    2. 室 |i| の使用するスケジュールの名称
    3. 1日あたりのステップ数, :math:`n_{step,day}`

また、使用するスケジュールは1日単位で別途設定しておく必要がある（後述）。

本節で示す計算の結果、次の値が得られる。

    1. ステップ |n| における室 |i| の人体発熱を除く内部発熱, :math:`q_{gen,i,n}`, W
    2. ステップ |n| における室 |i| の人体発湿を除く内部発湿, :math:`X_{gen,i,n}`, kg / s
    3. ステップ |n| における室 |i| の局所換気量, :math:`V_{mec,vent,local,i,n}`, |m3| / s
    4. ステップ |n| における室 |i| の在室人数, :math:`n_{hum,i,n}`, 人
    5. ステップ |n| における室 |i| の空調需要, :math:`r_{AC,demand,i,n}`, -
    6. ステップ |n| における室 |i| の運転モード, :math:`T_{AC,mode,i,n}`, -

ステップ |n| における室 |i| の空調需要, :math:`r_{AC,demand,i,n}` は、
ステップ |n| の時間間隔に対してどの程度の割合運転を行うのかを表す係数であり、0以上1以下の値である。
ステップ |n| における室 |i| の運転モード, :math:`T_{AC,mode,i,n}` は、
運転モードを表し、0以上の整数で表される。
運転モード「0」は、運転しないを表し、それ以外の運転モードは別途定義される。
ここで運転モードは、本節の外で自由に定義することができるが、
例えば制御を行うパラメータ（室温・作用温度・PMVなど）とその目標値（室温の場合は設定温度）などである。
なお、ここで定める運転モードは、暖房・冷房の区別は無いので、暖房の場合の挙動と冷房の場合の挙動をセットで外部で定義する必要がある。

------------------------------------------------------------------------------------------------------------------------
2 記号及び添え字
------------------------------------------------------------------------------------------------------------------------

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2.1 記号
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

この計算で用いる記号及び単位を次に示す。

.. list-table:: 表1 記号及び単位
    :header-rows: 1
    :widths: 2,6,1

    * - 記号
      - 意味
      - 単位
    * - :math:`A_{f,i}`
      - floor area of room |i| / 室 |i| の床面積
      - |m2|
    * - :math:`A_{f,total}`
      - total floor area / 床面積の合計
      - |m2|
    * - :math:`n_{hum,i,n}`
      - number of occupants in room |i| at step |n| / ステップ |n| における室 |i| の在室人数
      - －
    * - :math:`\hat{n}_{hum,i,n}`
      - number of occupants in room |i| at step |n| / ステップ |n| における室 |i| の在室人数
      - －
    * - :math:`n_{hum,T_{scd,d},i,n_d}`
      - number of occupants of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の在室人数
      - －
    * - :math:`n_{p,calc}`
      - number of occupant for calculation / 計算で用いる居住人数
      - －
    * - :math:`n_{step,day}`
      - number of steps in a day / 1 日あたりのステップ数
      - －
    * - :math:`\hat{q}_{gen,app,i,n}`
      - appliance heat generation in room |i| at step |n| / ステップ |n| における室 |i| の機器発熱量
      - W
    * - :math:`q_{gen,app,T_{scd,d},i,n_d}` 
      - appliance heat generation of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の機器発熱量
      - W
    * - :math:`\hat{q}_{gen,ckg,i,n}`
      - cooking heat generation in room |i| at step |n| / ステップ |n| における室 |i| の調理発熱量
      - W
    * - :math:`q_{gen,ckg,T_{scd,d},i,n_d}`
      - cooking heat generation of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の調理発熱量
      - W
    * - :math:`q_{gen,i,n}`
      - internal heat generation excluding human body heat generation in room |i| at step |n| / ステップ |n| における室 |i| の人体発熱を除く内部発熱
      - W
    * - :math:`\hat{q}_{gen,lght,i,n}`
      - lighting heat generation in room |i| at step |n| / ステップ |n| における室 |i| の照明発熱量
      - W / |m2|
    * - :math:`q_{gen,lght,T_{scd,d},i,n_d}`
      - lighting heat generation of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の照明発熱量
      - W
    * - :math:`r_{AC,demand,i,n}`
      - air conditioning demand ratio in room |i| at step |n| / ステップ |n| における室 |i| の空調割合
      - －
    * - :math:`\hat{r}_{AC,demand,i,n}`
      - air conditioning demand ratio in room |i| at step |n| / ステップ |n| における室 |i| の空調割合
      - －
    * - :math:`r_{AC,demand,T_{scd,d},i,n_d}`
      - air conditioning demand ratio of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の空調割合
      - －
    * - :math:`T_{AC,mode,i,n}`
      - air conditioning mode in room |i| at step |n| / ステップ |n| における室 |i| の空調モード
      - －
    * - :math:`\hat{T}_{AC,mode,i,n}`
      - air conditioning mode in room |i| at step |n| / ステップ |n| における室 |i| の空調モード
      - －
    * - :math:`T_{AC,mode,T_{scd,d},i,n_d}`
      - air conditioning mode of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の空調モード
      - －
    * - :math:`T_{scd,d}`
      - schedule pattern in day |d| / 日付 |d| のスケジュールパターン
      - －
    * - :math:`V_{mec,vent,local,i,n}`
      - local ventilation amount in room |i| at step |n| / ステップ |n| における室 |i| の局所換気量
      - |m3| / h
    * - :math:`\hat{V}_{mec,vent,local,i,n}`
      - local ventilation amount in room |i| at step |n| / ステップ |n| における室 |i| の局所換気量
      - |m3| / h
    * - :math:`V_{mec,vent,local,T_{scd,d},i,n_d}`
      - local ventilation amount of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の局所換気量
      - |m3| / h
    * - :math:`\hat{X}_{gen,ckg,i,n}`
      - cooking vapour generation in room |i| at step |n| / ステップ |n| における室 |i| の調理発湿量
      - g / h
    * - :math:`X_{gen,ckg,T_{scd,d},i,n_d}`
      - cooking vapour generation of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の調理発湿量
      - g / h
    * - :math:`X_{gen,i,n}`
      - internal vapour generation excluding human body vapour generation in room |i| at step |n| / ステップ |n| における室 |i| の人体発湿を除く内部発湿
      - kg / s
    * - :math:`x_{T_p,T_{scd,d},i,n_d}`
      - value of schedule pattern :math:`T_{scd,d}` and type of occupants :math:`T_p` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値( :math:`x` )
      - DOT
    * - :math:`x_{T_{scd,d},i,n_d}`
      - value of schedule pattern :math:`T_{scd,d}` in room |i| at step :math:`n_d` in day |d| / 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値( :math:`x` )
      - DOT

※ DOT = 値の種類による。


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2.2 添え字
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

この計算で用いる添え字を次に示す。

.. list-table:: 表2 添え字
    :header-rows: 1
    :widths: 1,7

    * - 添え字
      - 意味
    * - |i|
      - 室
    * - |n|
      - ステップ
    * - :math:`n_d`
      - 日付 |d| のステップ
    * - :math:`T_{scd,d}`
      - 日付 |d| のスケジュールパターン
    * - :math:`T_p`
      - 居住人数タイプ

------------------------------------------------------------------------------------------------------------------------
3 読み込むスケジュールの定義
------------------------------------------------------------------------------------------------------------------------

読み込むスケジュールは居住人数ごとに（ただし、居住人数に依存する使用スケジュールの場合のみ）及びスケジュールパターンごとに1日単位で7種類の値が定義される。

1日単位で定義される値は配列で定義され、その配列1日あたりのステップ数の長さを持つ。
例えば、15分間隔で指定する場合、配列の長さは96である。

スケジュールパターンは「平日」「休日外出」「休日在宅」の3種類のパターンが存在する。

使用スケジュールの指定方法は json 形式で指定する。

居住人数に依存するスケジュールの場合

.. code-block::

    {
        "4": {
            "Weekday": {
                "number_of_people": [0, 0, ..., 0],
                "heat_generation_appliances": [135, 135, ..., 135],
                "heat_generation_lighting": [0, 0, ..., 0],
                "heat_generation_cooking": [0, 0, ..., 0],
                "vapor_generation_cooking": [0, 0, ..., 0],
                "local_vent_amount": [0, 0, ..., 0],
                "is_temp_limit_set": [0, 0, ..., 0]
            },
            "Holiday_In": {
                "number_of_people": [0, 0, ..., 0],
                ...
            },
            "Holiday_Out": {
                "number_of_people": [0, 0, ..., 0],
                ...
            }
        },
        "3": {
            "Weekday": {
                ...
            },
            "Holiday_In": {
                ...
            },
            "Holiday_Out": {
                ...
            }
        },
        "2": {
            "Weekday": {
                ...
            },
            "Holiday_In": {
                ...
            },
            "Holiday_Out": {
                ...
            }
        },
        "1": {
            "Weekday": {
                ...
            },
            "Holiday_In": {
                ...
            },
            "Holiday_Out": {
                ...
            }
        }
    }

居住人数に依存しないスケジュールの場合

.. code-block::
    
    {
        "const": {
            "Weekday": {
                "number_of_people": [0, 0, ..., 0],
                "heat_generation_appliances": [135, 135, ..., 135],
                "heat_generation_lighting": [0, 0, ..., 0],
                "heat_generation_cooking": [0, 0, ..., 0],
                "vapor_generation_cooking": [0, 0, ..., 0],
                "local_vent_amount": [0, 0, ..., 0],
                "is_temp_limit_set": [0, 0, ..., 0]
            },
            "Holiday_In": {
                "number_of_people": [0, 0, ..., 0],
                ...
            },
            "Holiday_Out": {
                "number_of_people": [0, 0, ..., 0],
                ...
            }
        },
    }


------------------------------------------------------------------------------------------------------------------------
4 ステップ |n| における室 |i| の値
------------------------------------------------------------------------------------------------------------------------

ステップ |n| における室 |i| の人体発熱を除く内部発熱 :math:`q_{gen,i,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        q_{gen,i,n} = \hat{q}_{gen,app,i,n} + \hat{q}_{gen,ckg,i,n} + \hat{q}_{gen,lght,i,n} \cdot A_{f,i}
        \tag{1}
    \end{align*}

ステップ |n| における室 |i| の人体発湿を除く内部発湿 :math:`X_{gen,i,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        X_{gen,i,n} = \frac{ \hat{X}_{gen,ckg,i,n} }{1000 \cdot 3600}
        \tag{2}
    \end{align*}

ステップ |n| における室 |i| の局所換気量 :math:`V_{mec,vent,local,i,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        V_{mec,vent,local,i,n} = \frac{ \hat{V}_{mec,vent,local,i,n} }{3600}
        \tag{3}
    \end{align*}

ステップ |n| における室 |i| の在室人数 :math:`n_{hum,i,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        n_{hum,i,n} = \hat{n}_{hum,i,n}
        \tag{4}
    \end{align*}

ステップ |n| における室 |i| の空調割合 :math:`r_{AC,demand,i,d,n_d}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        r_{AC,demand,i,n} = \hat{r}_{AC,demand,i,n}
        \tag{5}
    \end{align*}

ステップ |n| における室 |i| の運転モード :math:`T_{AC,mode,i,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        T_{AC,mode,i,n} = \hat{T}_{AC,mode,i,n}
        \tag{6}
    \end{align*}

------------------------------------------------------------------------------------------------------------------------
5 ステップ |n| における室 |i| の特定の居住人数における値
------------------------------------------------------------------------------------------------------------------------

1年間1日ごとに定義された、日付 |d| のスケジュールパターン :math:`T_{scd,d}` (長さ365) に応じて、
日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_{scd,d},i,n_d}`
をつなげていく。

つなげた値を、ステップ |n| における室 |i| の値 :math:`x_{i,n}` とする。

ここで、日付 |d| のステップ :math:`n_d` の数（24, 48 又は 96）と年間日数（365） を乗じた値が ステップ |n| の値となる。

ここで、作成される値は次の8種類である。なお、json ファイルから読み込んだ値と、本プログラムで使用する値の単位が異なる項目が一部あるため、
ここで集約された値の記号は、:math:`\hat{}` (ハット) を付記する。

    1. ステップ |n| における室 |i| の局所換気量 :math:`\hat{V}_{mec,vent,local,i,n}`, |m3| / h
    2. ステップ |n| における室 |i| の機器発熱量 :math:`\hat{q}_{gen,app,i,n}`, W
    3. ステップ |n| における室 |i| の調理発熱量 :math:`\hat{q}_{gen,ckg,i,n}`, W
    4. ステップ |n| における室 |i| の調理発湿量 :math:`\hat{X}_{gen,ckg,i,n}`, g / h
    5. ステップ |n| における室 |i| の照明発熱量 :math:`\hat{q}_{gen,lght,i,n}`, W / |m2|
    6. ステップ |n| における室 |i| の在室人数 :math:`\hat{n}_{hum,i,n}`, ー
    7. ステップ |n| における室 |i| の空調割合 :math:`\hat{r}_{ac,demmand,i,n}`, ー
    8. ステップ |n| における室 |i| の空調モード :math:`\hat{T}_{ac,mode,i,n}`, ー

------------------------------------------------------------------------------------------------------------------------
6 スケジュールの読み込みと按分方法
------------------------------------------------------------------------------------------------------------------------

ここで作成する値は次の8種類である。

    1. 局所換気量(LOCAL_VENTILATION_AMMOUNT) :math:`V_{mec,vent,local}`, |m3| / h
    2. 機器発熱量(APPLIANCE_HEAT_GENERATION) :math:`q_{gen,app}`, W
    3. 調理発熱量(COOKING_HEAT_GENERATION) :math:`q_{gen,ckg}`, W
    4. 調理発湿量(COOKING_VAPOUR_GENERATION) :math:`X_{gen,ckg}`, g / h
    5. 照明発熱量(LIGHTING_HEAT_GENERATION) :math:`q_{gen,lght}`, W / |m2|
    6. 在室人数(NUMBER_OF_PEOPLE) :math:`n_{hum}`, ー
    7. 空調割合(0から1の間の小数)(AC_DEMMAND) :math:`r_{AC,demmand}`
    8. 空調モード(0から始まる整数)(AC_MODE) :math:`T_{AC,setting}`

これらの値を一般化して :math:`x` と記す。つまり、

- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の局所換気量 :math:`V_{mec,vent,local,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の機器発熱量 :math:`q_{gen,app,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の調理発熱量 :math:`q_{gen,ckg,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の調理発湿量 :math:`X_{gen,ckg,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の照明発熱量 :math:`q_{gen,lght,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の在室人数 :math:`n_{hum,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の空調割合 :math:`r_{ac,demmand,T_{scd,d},i,n_d}`
- 日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の空調モード :math:`T_{ac,mode,T_{scd,d},i,n_d}`

を、まとめて、

日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_{scd,d},i,n_d}` と記す。

"schedule type" が "const" の場合：

日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_{scd,d},i,n_d}` は、
日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_p,T_{scd,d},i,n_d}` の値に等しい。
ただし、:math:`T_p = \text{const}` である。

"schedule type" が "number" の場合：

日付 |d| のステップ :math:`n_d` における室 |i| の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_{scd,d},i,n_d}` は、
最大で2種類の日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_p,T_{scd,d},i,n_d}` の値から計算される。
ここで、:math:`T_p` は、計算で用いる居住人数 :math:`n_{p,calc}` に応じて次のように定まる。

.. math::
    :nowrap:

    \begin{align*}
        t_p = \begin{cases}
            1 & ( n_{p,calc} = 1 ) \\
            1, 2 & ( 1 < n_{p,calc} < 2 ) \\
            2 & ( n_{p,calc} = 2 ) \\
            2, 3 & ( 2 < n_{p,calc} < 3 ) \\
            3 & ( n_{p,calc} = 1 ) \\
            3, 4 & ( 3 < n_{p,calc} < 4 ) \\
            4 & ( n_{p,calc} = 1 ) \\
        \end{cases}
        \tag{6}
    \end{align*}

2種類の値を採用した場合、つまり、:math:`T_p` が、1と2 の組み合わせ、2と3 の組み合わせ、3と4 の組み合わせの場合の按分方法は、次節以降の「按分方法」に記す。

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値 :math:`x_{T_p,T_{scd,d},i,n_d}` の求め方を次節以降に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.1 局所換気量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の局所換気量 :math:`V_{mec,vent,local,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "local_vent_amount": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.2 機器発熱量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の機器発熱量 :math:`q_{gen,app,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "heat_generation_appliances": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.3 調理発熱量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の調理発熱量 :math:`q_{gen,ckg,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "heat_generation_cooking": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.4 調理発湿量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の調理発湿量 :math:`X_{gen,ckg,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "vapor_generation_cooking": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.5 照明発熱量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の照明発熱量 :math:`q_{gen,lght,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "heat_generation_lighting": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.6 在室人数
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の在室人数 :math:`n_{hum,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "number_of_people": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.7 空調割合
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の空調割合 :math:`r_{ac,demmand,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

ここで定義される "is_temp_limit_set" は、空調モードを表す。空調モードが "0" の場合は空調停止を表し、空調モードが "0" 以外の場合は、別途モード別に定義された条件で空調を行うことを意味する。
従って、ここで定義される "is_temp_limit_set" の値が "0" の場合は、

:math:`r_{ac,demmand,T_p,T_{scd,d},i,n_d} = 0`

とし、"is_temp_limit_set" の値が "0" 以外の場合は、

:math:`r_{ac,demmand,T_p,T_{scd,d},i,n_d} = 1`

とする。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "is_temp_limit_set": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.8 空調モード
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の空調モード :math:`T_{ac,mode,T_p,T_{scd,d},i,n_d}` は、
辞書 "schedule" の中で次のように定義される。

.. code-block::

    {
        "[居住人数タイプ]": {
            "[スケジュールパターン]": {
                "is_temp_limit_set": [1日あたりのステップ数の長さの配列] ※
            }
        }
    }

※ この部分が配列でない場合の処理の仕方を「6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法」に記す。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.9 按分方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

居住人数を指定しない場合の按分方法を記す。

按分は、計算で用いる居住人数 :math:`n_{p,calc}` に応じて行う。

計算で用いる居住人数 :math:`n_{p,calc}` は必ずしも整数にはならないため、スケジュールデータの何らかの合成方法が必要となる。

スケジュールの種類が、「局所換気量」、「機器発熱量」、「調理発熱量」、「調理発湿量」、「照明発熱量」、「在室人数」および「空調割合」の場合は、
次の按分方法を行う。

.. math::
    :nowrap:

    \begin{align*}
        x_{T_{scd,d},i,n_d} = \begin{cases}
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=1} & ( n_{p,calc} \le 1 ) \\
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=2} \cdot (n_{p,calc} - 1) + x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=1} \cdot (2 - n_{p,calc}) & ( 1 < n_{p,calc} \le 2 ) \\
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=3} \cdot (n_{p,calc} - 2) + x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=2} \cdot (3 - n_{p,calc}) & ( 2 < n_{p,calc} \le 3 ) \\
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=4} \cdot (n_{p,calc} - 3) + x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=3} \cdot (4 - n_{p,calc}) & ( 3 < n_{p,calc} \le 4 ) \\
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=4} & ( 4 < n_{p,calc} ) \\
        \end{cases}
        \tag{7}
    \end{align*}

スケジュールの種類が「空調モード」の場合は、次の按分方法を行う。

.. math::
    :nowrap:

    \begin{align*}
        x_{T_{scd,d},i,n_d} = \begin{cases}
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=1} & ( n_{p,calc} \le 1 ) \\
            \max( x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=1}, x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=2} ) & ( 1 < n_{p,calc} \le 2 ) \\
            \max( x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=2}, x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=3} ) & ( 2 < n_{p,calc} \le 3 ) \\
            \max( x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=3}, x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=4} ) & ( 3 < n_{p,calc} \le 4 ) \\
            x_{T_p,T_{scd,d},i,n_d} \mid_{T_p=4} & ( 4 < n_{p,calc} )
        \end{cases}
        \tag{8}
    \end{align*}

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6.10 schedule の読み込み・配列数の確認・指定が無い場合の作成方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

日付 |d| のステップ :math:`n_d` における室 |i| の
居住人数タイプ :math:`T_p` の日付 |d| のスケジュールパターン :math:`T_{scd,d}` の値
（この値は居住人数、家電発熱等、様々な種類の値が入る可能性があるため、ここでは仮に記号を :math:`x` とおく） 
:math:`x_{T_p,T_{scd,d},i,n_d}` は基本的に以下のように与えられる。

.. code-block::
	
    {
    	"number_of_people": [...],
		"heat_generation_appliances": [...],
		"heat_generation_lighting": [...],
		"heat_generation_cooking": [...],
		"vapor_generation_cooking": [...],
		"local_vent_amount": [...],
		"is_temp_limit_set": [...]
    }

ただし、該当するスケジュール名称（"number_of_people", "heat_generation_appliances", ...）が存在しない場合は、
値 :math:`0` で満たした配列を作成する。

また、値がリストではなくて、"zero" と与えられる、つまり次のような場合についても、値 :math:`0` で満たした配列を作成する。

.. code-block::
	
    {
    	"number_of_people": "zero",
        ...
    }


------------------------------------------------------------------------------------------------------------------------
8 schedule type と schedule のロード
------------------------------------------------------------------------------------------------------------------------

input.json ファイルには、最大で次の3つの項目が定義される。

    - name （必須項目）
    - schedule type （任意項目）
    - schedule （任意項目："schedule type" が定義されている場合は定義されないといけない。）

"schedule type" が定義されている場合は "schedule" の項目を読み込む。

"schedule type" が定義されていない場合は、"name" で指定された json ファイルから "schedule type" と "schedule" を読み込む。
その場合の json ファイルのフォーマットは次のようになっている。

.. code-block::
    
    {
        "schedule_type": ...,
        "schedule": ...
    }

"schedule" に含まれる項目・フォーマットは、「3 使用するスケジュール」を参照のこと。

"schedule type" は、次のいずれかが指定される。

number
    | 居住人数に応じてスケジュールが変わる場合に指定される。"number" が指定された場合は、スケジュールとして、1人、2人、3人、4人の4種類のスケジュールを用意しないといけない。
const
    | 居住人数に応じてスケジュールが変わらない場合に指定される。この場合、"const" スケジュール1つを指定しないといけない。

------------------------------------------------------------------------------------------------------------------------
9 計算で用いる居住人数
------------------------------------------------------------------------------------------------------------------------

1年間1日ごとに定義された、日付 |d| のスケジュールパターン :math:`T_{scd,d}` は次表により与えられる。

.. code-block:: python

    [
        "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", 
        "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", 
        "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "HI", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", 
        "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", 
        "W", "W", "HO", "HO", "HI", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", 
        "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", 
        "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "HI", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", 
        "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", 
        "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "HO", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", 
        "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "HI", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", 
        "W", "W", "HI", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "HI", "W", "HI", "HO", "W", "W", "W", "W", 
        "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HI", "HI", "W", "W", "W", "W", "W", "HI", "HO", "W", "W", "W", "W", "W", "HO", "HI"
    ]

ここで、"W"は「平日」、"HI"は「休日在宅」、"HO"は「休日外出」である。

------------------------------------------------------------------------------------------------------------------------
10 計算で用いる居住人数
------------------------------------------------------------------------------------------------------------------------

計算で用いる居住人数 :math:`n_{p,calc}` は、
「1人・2人・3人・4人の中から居住人数を直接指定する方法」を採用する場合は、そこで指定した居住人数とし、
「床面積から仮想的な居住人数を計算する方法」を採用する場合は、次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        n_{p,calc} = \begin{cases}
            30 & ( A_{f,total} < 30 ) \\
            \frac{A_{f,total}}{30} & ( 30 \le A_{f,total} < 120 ) \\
            120 & ( 120 \le A_{f,total} ) \\
        \end{cases}
        \tag{9}
    \end{align*}

床面積の合計 :math:`A_{f,total}` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        A_{f,total} = \sum_i^I{A_{f,i}}
        \tag{10}
    \end{align*}

