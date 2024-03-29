.. include:: definition.txt

.. raw:: latex

    \clearpage

========================================================================================================================
建物全般
========================================================================================================================

------------------------------------------------------------------------------------------------------------------------
1 はじめに
------------------------------------------------------------------------------------------------------------------------

本節では、繰り返し計算を行う前に取得すべき建物全般のパラメータと、
繰り返し計算時に使用される漏気量の計算方法について記述する。


次の値を必要とする。

＜繰り返し計算を行う前＞

    1. 入力ファイルにおける building 要素
  
＜繰り返し計算時＞

    2. ステップ |n| における室 |i| の空気温度 :math:`\theta_{r,i,n}`
    3. ステップ |n| における外気温度 :math:`\theta_{o,n}`
    4. 室 |i| の容積 :math:`V_{r,i}`

次の項目が得られる。

    1. ステップ |n| における室 |i| のすきま風量 :math:`V_{leak,i,n}`

------------------------------------------------------------------------------------------------------------------------
2 記号及び添え字
------------------------------------------------------------------------------------------------------------------------

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2.1 記号
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

この計算で用いる記号及び単位を次に示す。

.. list-table:: 表1 記号及び単位
    :header-rows: 1
    :widths: 1,6,1

    * - 記号
      - 意味
      - 単位
    * - :math:`C`
      - C value / 相当隙間面積（C値）
      - |cm2| / |m2|
    * - :math:`n_{leak,n}`
      - ventilation rate of air leakage at step n / ステップ |n| におけるすきま風による住宅全体の換気回数
      - 回 / h
    * - :math:`U_A`
      - U\ :sub:`A` \ value / U\ :sub:`A` \ 値
      - W / ( |m2| K)
    * - :math:`V_{leak,i,n}`
      - leakage air volume of room i at step n / ステップ |n| における室 |i| のすきま風量
      - |m3| / s
    * - :math:`V_{r,i}`
      - volume of room |i| / 室 |i| の容積
      - |m3|
    * - :math:`\theta_o`
      - outside air temperature at step |n| / ステップ |n| における外気温度
      - ℃
    * - :math:`\theta_{r,i,n}`
      - air temperature in room i at step n / ステップ |n| における室 |i| の空気温度
      - ℃
    * - :math:`\Delta \theta_n`
      - air temperature difference between room and outside at step n / ステップ |n| における室内外空気温度差
      - K
    * - :math:`\bar{\theta}_{r}`
      - average air temperature in building at step |n| / ステップ |n| における建物の平均空気温度
      - ℃

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

------------------------------------------------------------------------------------------------------------------------
3 気密の指定方法
------------------------------------------------------------------------------------------------------------------------

building 要素の infiltration 要素の method 要素で指定される値とする。

なお、現状、"balance_residential" =「住宅用」 のみが指定される。

後々、「非住宅用」あるいは「マンション」等の大幅に建て方が異なる場合に備えて項目を設けている。

------------------------------------------------------------------------------------------------------------------------
4 建物の階数
------------------------------------------------------------------------------------------------------------------------

building 要素の infiltration 要素の story 要素で指定される値とする。

------------------------------------------------------------------------------------------------------------------------
5 C値
------------------------------------------------------------------------------------------------------------------------

building 要素の infiltration 要素の c_value_estimate 要素が、"specify" か "calculate" かで指定方法が異なる。

c_value_estimate 要素が、"specify" の場合、building 要素の infiltration 要素の c_value の値とする。

c_value_estimate 要素が、"calculate" の場合、
building 要素の infiltration 要素の ua_value 要素の値と
building 要素の infiltration 要素の struct 要素の値から計算するとし、
C値 :math:`C` は次の式で表される。

.. math::
    :nowrap:

    \begin{align*}
        C = a \cdot U_A
        \tag{1}
    \end{align*}

|   :math:`a` : U\ :sub:`A` \ 値からC値に換算する係数（表2）, ( |cm2| / |m2| ) / ( W / ( |m2| K ) )

U\ :sub:`A` \ 値からC値に換算する係数 :math:`a` は建物の構造に依存し次表から求める。

.. list-table:: 表2 U\ :sub:`A` \ 値からC値に換算する係数
    :header-rows: 1
    :widths: 1,4

    * - 建物の構造
      - U\ :sub:`A` \ 値からC値に換算する係数 :math:`a`, ( |cm2| / |m2| ) / ( W / ( |m2| K ) )
    * - RC造
      - 4.16
    * - SRC造
      - 4.16
    * - 木造
      - 8.28
    * - 鉄骨造
      - 8.28

建物の構造は、building 要素の infiltration 要素の struct 要素が、
"rc" の場合はRC造、
"src" の場合はSRC造、
"wooden" の場合は木造、
"steel" の場合は鉄骨造
とする。

------------------------------------------------------------------------------------------------------------------------
6 すきま風量
------------------------------------------------------------------------------------------------------------------------

ステップ |n| における室 |i| のすきま風量 :math:`V_{leak,i,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        V_{leak,i,n} = \frac{ n_{leak,n} \cdot V_{r,i} }{3600}
        \tag{2}
    \end{align*}

ステップ |n| のすきま風による住宅全体の換気回数 :math:`n_{leak,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        n_{leak,n} = \max \{ b_1 \cdot C \cdot \sqrt{\Delta \theta_n} - b_2, 0 \}
        \tag{3}
    \end{align*}

|   :math:`b_1` : 近似式の係数, 回 /  (h ( |cm2| / |m2| ) K \ :sup:`0.5`\ )
|   :math:`b_2` : 近似式の係数, 回 / h

近似式の係数 :math:`b_1` は階数で決定される係数であり、1階建ての時に :math:`0.022` を、2階建ての時に :math:`0.020` をとる。

近似式の係数 :math:`b_2` は階数と換気方式の組み合わせで決定される係数であり、
室内の圧力が高くなる第二種換気では、1階建てで :math:`0.26` 、2階建てで :math:`0.14` とする。
室内の圧力が低くなる第三種換気では、1階建てで :math:`0.28` 、2階建てで :math:`0.13` とする。
内外差圧がバランスしている第一種換気では、階数によらず :math:`0.0` とする。

ステップ |n| における室内外空気温度差 :math:`\Delta \theta_n` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        \Delta \theta_n = | \bar{\theta}_{r,n} - \theta_{o,n} |
        \tag{4}
    \end{align*}

ステップ |n| における建物の平均空気温度 :math:`\bar{\theta}_{r,n}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        \bar{\theta}_{r,n} = \dfrac{\sum\limits_{i} V_{r,i} \cdot \theta_{r,i,n}}{\sum\limits_{i} V_{r,i}}
        \tag{5}
    \end{align*}

