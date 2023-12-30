.. include:: definition.txt

.. raw:: latex

    \clearpage

========================================================================================================================
太陽位置
========================================================================================================================

------------------------------------------------------------------------------------------------------------------------
1 はじめに
------------------------------------------------------------------------------------------------------------------------

本節では、緯度、経度、時間間隔（15分間隔・30分間隔・1時間間隔）が指定された場合の任意のステップの太陽位置の求め方について定義する。

本節の計算は、次の値を必要とする。

    1. 緯度 :math:`\phi_{loc}`, rad
    2. 経度 :math:`\lambda_{loc}`, rad
    3. 時間間隔（15分間隔・30分間隔・1時間間隔）
    4. 1 時間を分割するステップ数 :math:`n_{hour}`

本節で示す計算の結果、次の値が得られる。

    1. ステップ |n| における太陽方位角 :math:`a_{sun,n}`, rad
    2. ステップ |n| における太陽高度 :math:`h_{sun,n}`, rad


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
    * - :math:`a_{sun,n}`
      - solar direction at step |n| / ステップ |n| における太陽方位角
      - rad
    * - :math:`\sin{a_{sun,n}}`
      - sine of solar direction at step |n| / ステップ |n| における太陽方位角の正弦
      - －
    * - :math:`\cos{a_{sun,n}}`
      - cosine of solar direction at step |n| / ステップ |n| における太陽方位角の余弦
      - －
    * - :math:`h_{sun,n}`
      - solar altitude at step |n| / ステップ |n| における太陽高度
      - rad
    * - :math:`\varphi_{loc}`
      - latitude / 緯度
      - rad
    * - :math:`\delta_{n}`
      - declination at step n / ステップ |n| における赤緯
      - rad
    * - :math:`\omega_{n}`
      - time angle at step |n| / ステップ |n| における時角
      - rad
    * - :math:`t_{m,n}`
      - standard time at step |n| / ステップ |n| における標準時
      - h
    * - :math:`\lambda_{loc}`
      - longitude / 経度
      - rad
    * - :math:`\lambda_{loc,mer}`
      - longitude of the standard meridian / 標準子午線の経度
      - rad
    * - :math:`e_{t,n}`
      - equation of time at step |n| / ステップ |n| における均時差
      - rad
    * - :math:`\nu_n`
      - true anomaly at step |n| / ステップ |n| における真近点離角
      - rad
    * - :math:`\epsilon_n`
      - angle between perihelion and winter solstice at step |n| / ステップ |n| における近日点と冬至点の角度
      - rad
    * - :math:`\delta_0`
      - northern hemisphere winter solstice day declination / 北半球の冬至の日赤緯
      - rad
    * - :math:`m_n`
      - mean anomaly at step |n| / ステップ |n| における平均近点離角
      - rad
    * - :math:`N`
      - annual difference to 1968 / 1968年との年差
      - 年
    * - :math:`n_{hour}`
      - number of steps to divide one hour / 1 時間を分割するステップ数
      - －
    * - :math:`d_n`
      - year total date at step |n| / ステップ |n| における年通算日( :math:`1` 月 :math:`1` 日を :math:`1` とする)
      - day
    * - :math:`d_0`
      - mean orbital perihelion passage date / 平均軌道上の近日点通過日(暦表時による :math:`1968` 年 :math:`1` 月 :math:`1` 日正午基準の日差)
      - day
    * - :math:`d_{ay}`
      - anomalistic year / 近点年(近日点基準の公転周期日数)
      - day
    * - :math:`y`
      - year of calculation / 計算する年(西暦)
      - 年

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2.2 添え字
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

この計算で用いる添え字を次に示す。

.. list-table:: 表2 添え字
    :header-rows: 1
    :widths: 1,7

    * - 添え字
      - 意味
    * - :math:`n`
      - step / ステップ

------------------------------------------------------------------------------------------------------------------------
3 太陽方位角・高度
------------------------------------------------------------------------------------------------------------------------

ステップ |n| における太陽方位角 :math:`a_{sun,n}` は、 :math:`-\pi` ～ :math:`0` ～ :math:`\pi` の範囲で定義され、:math:`a_{sun,n}=\pm \pi` を北、 :math:`a_{sun,n}=-\pi/2` を東、 :math:`a_{sun,n}=0` を南、 :math:`a_{sun,n}=\pi/2` を西として、式(1)により計算される。
ただし、太陽の位置が天頂にある場合は定義されない。

.. math::
    :nowrap:

    \begin{align*}
        a_{sun,n} = arctan( \sin{a_{sun,n}},\cos{a_{sun,n}} ) \tag{1}
    \end{align*}


:math:`arctan(y, x)` は、 :math:`-\pi/2` ～ :math:`0` ～ :math:`\pi/2` の範囲で定義される通常の逆正接関数とは異なり、座標上で第1引数を :math:`y` , 第2引数を :math:`x` にした際に :math:`x` 軸との角度を求める関数として、 :math:`-\pi \leq arctan(y, x) \leq \pi` の範囲で定義される。すなわち、 :math:`\sin{a_{sun,n}}>0` かつ :math:`\cos{a_{sun,n}}>0` の場合は :math:`0` ～ :math:`\pi/2` の角度を、:math:`\sin{a_{sun,n}}>0` かつ :math:`\cos{a_{sun,n}}<0` の場合は :math:`\pi/2` ～ :math:`\pi` の角度を、 :math:`\sin{a_{sun,n}}<0` かつ :math:`\cos{a_{sun,n}}<0` の場合は :math:`-\pi` ～ :math:`-\pi/2` の角度を、 :math:`\sin{a_{sun,n}}<0` かつ :math:`\cos{a_{sun,n}}>0` の場合は :math:`-\pi/2` ～ :math:`0` の角度を返す関数となる。
 
ステップ |n| における太陽方位角の余弦 :math:`\cos{a_{sun,n}}` は、次式により計算される。
ただし、太陽の位置が天頂にある場合は定義されない。

.. math::
    :nowrap:

    \begin{align*}
        \cos{a_{sun,n}} = \dfrac{ \sin{h_{sun,n}} \cdot \sin{\varphi_{loc}} - \sin{\delta_{n}} }{ \cos{h_{sun,n}} \cdot \cos{\varphi_{loc}} } \tag{2}
    \end{align*}

ステップ |n| における太陽の方位角の正弦 :math:`\sin{a_{sun,n}}` は、次式により計算される。
ただし、太陽の位置が天頂にある場合は定義されない。

.. math::
    :nowrap:

    \begin{align*}
        \sin{a_{sun,n}} = \dfrac{ \cos{\delta_{n}} \cdot \sin{\omega_{n}} }{ \cos{h_{sun,n}} } \tag{3}
    \end{align*}

太陽の位置が天頂にある場合とは、太陽高度 :math:`h_{sun,n}` が次式を満たす場合を言う。

.. math::
    :nowrap:

    \begin{align*}
        h_{sun,n} = \frac{ \pi }{ 2 }
    \end{align*}


ステップ |n| における太陽高度 :math:`h_{sun,n}` は、 :math:`-\pi/2 \leq h_{sun,n} \leq \pi/2` の範囲で次式により計算される。
    
 .. math::
    :nowrap:

    \begin{align*}
        h_{sun,n} = \arcsin( \sin{\varphi_{loc}} \cdot \sin{\delta_{n}} + \cos{\varphi_{loc}} \cdot \cos{\delta_{n}} \cdot \cos{\omega_{n}} ) \tag{4}
    \end{align*}

なお、 :math:`h_{sun,n} < 0` は、太陽が沈んでいることを意味する。

ステップ |n| における時角 :math:`\omega_{n}` は、次式により計算される。
    
.. math::
    :nowrap:

    \begin{align*}
        \omega_{n} = \{ ( t_{m,n} - 12 ) \cdot 15 \} \cdot \dfrac{\pi}{180} + ( \lambda_{loc} - \lambda_{loc,mer} ) + e_{t,n}
        \tag{5}
    \end{align*}   

ステップ |n| における標準時 :math:`t_{m,n}` は、時間間隔により次表のように定義される。

.. csv-table:: 時間間隔が1時間の場合

     ステップ |n| , 0, 1, 2, …, 8760
     標準時 , 0, 1, 2, …, 8760

.. csv-table:: 時間間隔が30分の場合

     ステップ |n| , 0, 1, 2, 3, 4, …, 17519, 17520
     標準時 , 0, 0.5, 1, 1.5, 2, …, 8759.5, 8760

.. csv-table:: 時間間隔が15分の場合

     ステップ |n| , 0, 1, 2, 3, 4, 5, 6, …, 35039, 35040
     標準時 , 0, 0.25, 0.5, 0.75, 1, 1.25, 1.5, …, 8759.75, 8760

ステップ |n| における赤緯 :math:`\delta_{n}` は、 :math:`-\pi/2 \leq \delta_{n} \leq \pi/2` の範囲で次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        \delta_{n} = \arcsin \{ \cos ( \nu_n + \epsilon_n ) \cdot \sin \delta_0 \}
        \tag{6}
    \end{align*}  

北半球の冬至の日赤緯 :math:`\delta_0` は、 :math:`-23.4393 \cdot \pi/180` radを用いる。

ステップ |n| における均時差 :math:`e_{t,n}` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        e_{t,n} = ( m_n - \nu_n ) - \arctan \dfrac{ 0.043 \sin \{ 2 ( \nu_n + \epsilon_n ) \} }{ 1 - 0.043 \cos \{ 2 ( \nu_n + \epsilon_n ) \} }
        \tag{7}
    \end{align*}  


ステップ |n| における真近点離角 :math:`\nu_n` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        \nu_n = m_n + ( 1.914 \sin m_n + 0.02 \sin 2m_n ) \cdot \dfrac{\pi}{180}
        \tag{8}
    \end{align*}

ステップ |n| における近日点と冬至点の角度 :math:`\epsilon_n` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        \epsilon_n = \Bigl\{ 12.3901 + 0.0172 \Bigl( N + \dfrac{ m_n }{ 2 \pi } \Bigr) \Bigr\} \cdot \dfrac{\pi}{180}
        \tag{9}
    \end{align*}


ステップ |n| における平均近点離角 :math:`m_n` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        m_n = 2 \pi ( d_n - d_0 ) / d_{ay}
        \tag{10}
    \end{align*}  
    
近点年(近日点基準の公転周期日数) :math:`d_{ay}` は :math:`365.2596` とする。

ステップ |n| における平均軌道上の近日点通過日(暦表時による :math:`1968` 年 :math:`1` 月 :math:`1` 日正午基準の日差) :math:`d_0` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        d_0 = 3.71 + 0.2596 N - \biggl\lfloor \dfrac{ N + 3 }{ 4 } \biggr\rfloor
        \tag{11}
    \end{align*}  

なお :math:`\lfloor x \rfloor` は、 :math:`x` の小数点以下を切り捨てた値とする。
	
1968年との年差 :math:`N` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        N = y - 1968
        \tag{12}
    \end{align*}  
    
本計算では、計算するする年は西暦1989年とする。
	
:math:`1` 月 :math:`1` 日を :math:`1` とする、ステップ |n| における年通算日 :math:`d_n` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        d_n = \biggl\lfloor \dfrac{ n }{ 24 \cdot n_{hour} } \biggr\rfloor + 1
        \tag{13}
    \end{align*} 

なお :math:`\lfloor x \rfloor` は、 :math:`x` の小数点以下を切り捨てた値とする。

標準子午線の経度 :math:`\lambda_{loc,mer}` は、次式により計算される。

.. math::
    :nowrap:

    \begin{align*}
        \lambda_{loc,mer} = 135 \cdot \dfrac{\pi}{180}
        \tag{14}
    \end{align*} 

