.. include:: definition.txt

.. raw:: latex

    \clearpage

========================================================================================================================
窓
========================================================================================================================

------------------------------------------------------------------------------------------------------------------------
1 はじめに
------------------------------------------------------------------------------------------------------------------------

本節では、窓のパラメータの計算方法について記述する。

本節の計算の結果、次の項目が得られる。

    1. ステップ |n| における境界 |j| の窓の直達日射に対する日射透過率 :math:`\tau_{w,d,j,n}`, -
    2. ステップ |n| における境界 |j| の窓の直達日射に対する吸収日射熱取得率 :math:`B_{w,d,j,n}`, -
    3. 境界 |j| の窓の天空日射に対する日射透過率 :math:`\tau_{w,s,j}`, -
    4. 境界 |j| の窓の地面反射日射に対する日射透過率 :math:`\tau_{w,r,j}`, -
    5. 境界 |j| の窓の天空日射に対する吸収日射熱取得率 :math:`B_{w,s,j}`, -
    6. 境界 |j| の窓の地面反射日射に対する吸収日射熱取得率 :math:`B_{w,r,j}`, -

本節の計算は次の値を必要とする。

    1. 境界 |j| の窓の熱損失係数(U値) :math:`U_{w,j}`, W / |m2| K
    2. 境界 |j| の日射熱取得率(η値) :math:`\eta_{w,j}`, -
    3. 境界 |j| の窓の面積に対するグレージングの面積の比 :math:`r_{A,w,g,j}`, -
    4. 境界 |j| の窓の建具(フレーム)材質の種類(樹脂・アルミニウム・アルミ樹脂複合) :math:`T_{w,flame}`
    5. 境界 |j| の窓の構成(単層・複層) :math:`T_{w,glass}`
    6. ステップ |n| における境界 |j| の窓に対する直達日射の入射角 :math:`\varphi_{j,n}`, rad

------------------------------------------------------------------------------------------------------------------------
2 直達日射に対する日射透過率・吸収日射熱取得率
------------------------------------------------------------------------------------------------------------------------

ステップ |n| における境界 |j| の窓の直達日射に対する日射透過率 :math:`\tau_{w,d,j,n}` は次式により表される。

.. math::
    :nowrap:
    
    \begin{align*}
        \tau_{w,d,j,n} = \tau_{w,j} (\phi_{j,n})
        \tag{1}
    \end{align*}

| ここで
|   :math:`\tau_{w,d,j,n}` : ステップ |n| における境界 |j| の窓の直達日射に対する日射透過率, -
|   :math:`\tau_{w,j} (\phi)` : 入射角 :math:`phi` に対する境界 |j| の窓の日射透過率（関数）, -
|   :math:`\phi_{j,n}` : ステップ |n| における境界 |j| の窓の直達日射の入射角, rad
| である。

ステップ |n| における境界 |j| の窓の直達日射に対する吸収日射熱取得率 :math:`B_{w,d,j,n}` は次式により表される。

.. math::
    :nowrap:
    
    \begin{align*}
        B_{w,d,j,n} = B_{w,j} (\phi_{j,n})
        \tag{2}
    \end{align*}

| ここで
|   :math:`B_{w,d,j,n}` : ステップ |n| における境界 |j| の窓の直達日射に対する吸収日射熱取得率, -
|   :math:`B_{w,j} (\phi)` : 入射角 :math:`phi` に対する境界 |j| の窓の吸収日射熱取得率（関数）, -
|   :math:`\phi_{j,n}` : ステップ |n| における境界 |j| の窓の直達日射の入射角, rad
| である。

------------------------------------------------------------------------------------------------------------------------
3 天空放射・地面反射日射に対する日射透過率・吸収日射熱取得率
------------------------------------------------------------------------------------------------------------------------

境界 |j| の窓の天空日射に対する日射透過率 :math:`\tau_{w,s,j}` 、
境界 |j| の窓の地面反射日射に対する日射透過率 :math:`\tau_{w,r,j}` 、
境界 |j| の窓の天空日射に対する吸収日射熱取得率 :math:`B_{w,s,j}` 、及び
境界 |j| の窓の地面反射日射に対する吸収日射熱取得率 :math:`B_{w,r,j}`
は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,s,j} = \tau_{w,c,j}
      \tag{3}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,r,j} = \tau_{w,c,j}
      \tag{4}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      B_{w,s,j} = B_{w,c,j}
      \tag{5}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      B_{w,r,j} = B_{w,c,j}
      \tag{6}
    \end{align*}

| ここで
|   :math:`\tau_{w,s,j}` : 境界 |j| の窓の天空日射に対する日射透過率, -
|   :math:`\tau_{w,r,j}` : 境界 |j| の窓の地面反射日射に対する日射透過率, -
|   :math:`\tau_{w,c,j}` : 境界 |j| の窓の1/4球の一様拡散日射に対する日射透過率, -
|   :math:`B_{w,s,j}` : 境界 |j| の窓の天空日射に対する吸収日射熱取得率, -
|   :math:`B_{w,r,j}` : 境界 |j| の窓の地面反射日射に対する吸収日射熱取得率, -
|   :math:`B_{w,c,j}` : 境界 |j| の窓の1/4球の一様拡散日射に対する吸収日射熱取得率, -
| である。

境界 |j| の窓の1/4球の一様拡散日射に対する日射透過率 :math:`\tau_{w,c,j}` 及び
境界 |j| の窓の1/4球の一様拡散日射に対する吸収日射熱取得率  :math:`B_{w,c,j}` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,c,j} = \frac {\pi}{M} \cdot \sum_{m=1}^M{ \tau_{w,j} (\phi_m) \cdot \sin \phi_m \cdot \cos \phi_m } 
      \tag{7}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      B_{w,c,j} = \frac {\pi}{M} \cdot \sum_{m=1}^M{ B_{w,j} (\phi_m) \cdot \sin \phi_m \cdot \cos \phi_m } 
      \tag{8}
    \end{align*}

| ここで
|   :math:`\tau_{w,c,j}` : 境界 |j| の窓の1/4球の一様拡散日射に対する日射透過率, -
|   :math:`B_{w,c,j}` : 境界 |j| の窓の1/4球の一様拡散日射に対する吸収日射熱取得率, -
|   :math:`\tau_{w,j} (\phi)` : 入射角 :math:`phi` に対する境界 |j| の窓の日射透過率（関数）, -
|   :math:`B_{w,j} (\phi)` : 入射角 :math:`phi` に対する境界 |j| の窓の吸収日射熱取得率（関数）, -
|   :math:`\phi_m` : 番号 :math:`m` の入射角, rad
| である。

また、

.. math::
    :nowrap:

    \begin{align*}
      \phi_m = \frac{\pi}{2} \cdot \frac{m - \frac{1}{2}}{M} 
    \end{align*}

    \begin{align*}
      m = 1, ... , M
    \end{align*}

    \begin{align*}
      M = 1000 
    \end{align*}

とする。

------------------------------------------------------------------------------------------------------------------------
4 任意の入射角に対する窓の日射透過率・吸収日射熱取得率
------------------------------------------------------------------------------------------------------------------------

入射角 :math:`\phi` に対する境界 |j| の窓の日射透過率（関数） :math:`\tau_{w,j} (\phi)` 及び
入射角 :math:`\phi` に対する境界 |j| の窓の吸収日射熱取得率（関数） :math:`B_{w,j} (\phi)` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,j} (\phi) = \tau_{w,g,j} (\phi) \cdot r_{A,w,g,j}
      \tag{9}
    \end{align*}

    \begin{align*}
      B_{w,j} (\phi) = B_{w,g,j} (\phi) \cdot r_{A,w,g,j}
      \tag{10}
    \end{align*}

| ここで
|   :math:`\tau_{w,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓の日射透過率（関数）, -
|   :math:`B_{w,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓の吸収日射熱取得率（関数）, -
|   :math:`\tau_{w,g,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射透過率（関数）, -
|   :math:`B_{w,g,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の吸収日射熱取得率（関数）, -
|   :math:`r_{A,w,g,j}` : 境界 |j| の窓の面積に対するガラスの面積の比, -
| である。

入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の吸収日射熱取得率 :math:`B_{w,g,j} (\phi)` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        B_{w,g,j} (\phi) = ( 1 - \tau_{w,g,j} (\phi) - \rho_{w,g,j} (\phi) ) \cdot r_{R,w,g,j}
      \tag{11}
    \end{align*}

| ここで
|   :math:`B_{w,g,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の吸収日射熱取得率, -
|   :math:`\tau_{w,g,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射透過率, -
|   :math:`\rho_{w,g,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射反射率, -
|   :math:`r_{R,w,g,j}` : 境界 |j| の窓のガラス部分の日射吸収量に対する室内側に放出される量の割合, -
| である。

入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射透過率 :math:`\tau_{w,g,j} (\phi)`  及び 
入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射反射率 :math:`\rho_{w,g,j} (\phi)` 
は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,g,j} (\phi) = \begin{cases}
        \tau_{w,g,s1,j} (\phi) & ( \text{単層の場合} ) \\
        \frac{ \tau_{w,g,s1,j} (\phi) \cdot \tau_{w,g,s2,j} (\phi) }{ 1 - \rho_{w,g,s1b,j} (\phi) \cdot \rho_{w,g,s2f,j} (\phi) } & ( \text{複層の場合} )
      \end{cases}
      \tag{12}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,j} (\phi) = \begin{cases}
        \rho_{w,g,s1f,j} (\phi) & ( \text{単層の場合} ) \\
        \rho_{w,g,s1f,j} (\phi) + \frac{ \tau_{w,g,s1,j} (\phi) \cdot \tau_{w,g,s2f,j} (\phi) \cdot \rho_{w,g,s2f,j} (\phi) }{ 1 - \rho_{w,g,s1b,j} (\phi) \cdot \rho_{w,g,s2f,j} (\phi) } & ( \text{複層の場合} )
      \end{cases}
      \tag{13}
    \end{align*}

ただし、

.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,s1b,j} (\phi) \cdot \rho_{w,g,s2f,j} (\phi) > 0.9999
    \end{align*}

の場合は、
 
.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,s1b,j} (\phi) \cdot \rho_{w,g,s2f,j} (\phi) = 0.9999
    \end{align*}
 
とする。（ゼロ割りの回避）

| ここで
|   :math:`\tau_{w,g,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射透過率（関数）, -
|   :math:`\rho_{w,g,j} (\phi)`	: 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の日射反射率（関数）, -
|   :math:`\tau_{w,g,s1,j} (\phi)`	: 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率, -
|   :math:`\tau_{w,g,s2,j} (\phi)`	: 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの透過率, -
|   :math:`\rho_{w,g,s1f,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側）, -
|   :math:`\rho_{w,g,s1b,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（背面側）, -
|   :math:`\rho_{w,g,s2f,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側）, -
| である。

入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率 :math:`\tau_{w,g,s1,j} (\phi)` 、
入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの透過率 :math:`\tau_{w,g,s2,j} (\phi)` 、
入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側） :math:`\rho_{w,g,s1f,j} (\phi)` 、
入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（背面側） :math:`\rho_{w,g,s1b,j} (\phi)` 、及び
入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側） :math:`\rho_{w,g,s2f,j} (\phi)`
は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,g,s1,j} (\phi) = \tau_{w,g,s1,j} \cdot \tau_n (\phi)
      \tag{14}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,g,s2,j} (\phi) = \tau_{w,g,s2,j} \cdot \tau_n (\phi)
      \tag{15}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,s1f,j} (\phi) = \rho_{w,g,s1f,j} + ( 1 - \rho_{w,g,s1f,j} ) \cdot \rho_n (\phi)
      \tag{16}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,s1b,j} (\phi) = \rho_{w,g,s1b,j} + ( 1 - \rho_{w,g,s1b,j} ) \cdot \rho_n (\phi)
      \tag{17}
    \end{align*}

.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,s2f,j} (\phi) = \rho_{w,g,s2f,j} + ( 1 - \rho_{w,g,s2f,j} ) \cdot \rho_n (\phi)
      \tag{18}
    \end{align*}


| ここで、
|   :math:`\tau_{w,g,s1,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率, -
|   :math:`\tau_{w,g,s2,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの透過率, -
|   :math:`\rho_{w,g,s1f,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側）, -
|   :math:`\rho_{w,g,s1b,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（背面側）, -
|   :math:`\rho_{w,g,s2f,j} (\phi)` : 入射角 :math:`\phi` に対する境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側）, -
|   :math:`\tau_{w,g,s1,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率, -
|   :math:`\tau_{w,g,s2,j}` : 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの透過率, -
|   :math:`\rho_{w,g,s1f,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側）, -
|   :math:`\rho_{w,g,s1b,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（背面側）, -
|   :math:`\rho_{w,g,s2f,j}` : 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側）, -
|   :math:`\tau_n (\phi)` : 入射角 :math:`\phi` に対する規準化透過率
|   :math:`\rho_n (\phi)` : 入射角 :math:`\phi` に対する規準化反射率
| である。

単層の場合は :math:`\tau_{w,g,s1,j} (\phi)` ・ :math:`\rho_{w,g,s1f,j} (\phi)` のみ計算すればよく、
その他の :math:`\tau_{w,g,s2,j} (\phi)` ・ :math:`\rho_{w,g,s1b,j} (\phi)` ・ :math:`\rho_{w,g,s2f,j} (\phi)` は複層のみ必要な値である。

入射角 :math:`\phi` に対する規準化透過率 :math:`\tau_n (\phi)` 及び
入射角 :math:`\phi` に対する規準化反射率 :math:`\rho_n (\phi)` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        \tau_n (\phi) = \sum_{k=0}^5 m_{\tau,k} \cdot \cos^k \phi
      \tag{19}
    \end{align*}

    \begin{align*}
        \rho_n (\phi) = \sum_{k=0}^5 m_{\rho,k} \cdot \cos^k \phi
      \tag{20}
    \end{align*}

| ここで、
|   :math:`\tau_n (\phi)` : 入射角 :math:`\phi` に対する規準化透過率, -
|   :math:`\rho_n (\phi)` : 入射角 :math:`\phi` に対する規準化反射率, -
|   :math:`\phi` : 入射角, rad
| である。

また、係数 :math:`m_{\tau,k}` 及び係数 :math:`m_{\rho,k}` は次表の値とする。

.. list-table:: 表1 係数 :math:`m_{\tau, k}` 及び係数 :math:`m_{\rho, k}` の値
    :header-rows: 1
    :widths: 1,1,1,1,1,1,1

    * - 係数
      - :math:`k=0`
      - :math:`k=1`
      - :math:`k=2`
      - :math:`k=3`
      - :math:`k=4`
      - :math:`k=5`
    * - :math:`m_{\tau, k}`
      - 0.0
      - 2.552
      - 1.364
      - -11.388
      - 13.617
      - -5.146
    * - :math:`m_{\rho, k}`
      - 1.000
      - -5.189
      - 12.392
      - -16.593
      - 11.851
      - -3.461

------------------------------------------------------------------------------------------------------------------------
5 各層の板ガラスの透過率・反射率
------------------------------------------------------------------------------------------------------------------------

次に各層の板ガラスの透過率・反射率の計算方法を示す。

単層の場合は :math:`\tau_{w,g,s1,j}` ・ :math:`\rho_{w,g,s1f,j}` のみ計算すればよく、その他の :math:`\tau_{w,g,s2,j}`・ :math:`\rho_{w,g,s1b,j}` ・ :math:`\rho_{w,g,s2f,j}` は複層のみ必要な値である。

境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（背面側） :math:`\rho_{w,g,s1b,j}` ・ 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの透過率 :math:`\tau_{w,g,s2,j}` は次式により表される。

（複層のみ）

.. math::
    :nowrap:

    \begin{align*}
      \rho_{w,g,s1b,j} = 0.379 \cdot ( 1 - \tau_{w,g,s1,j} ) 
      \tag{21}
    \end{align*}

    \begin{align*}
      \tau_{w,g,s2,j} = \tau_{w,g,s1,j}
      \tag{22}
    \end{align*}

| ここで
|   :math:`\rho_{w,g,s1b,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（背面側）, -
|   :math:`\tau_{w,g,s2,j}` : 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの透過率, -
|   :math:`\tau_{w,g,s1,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率, -
| である。

境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率 :math:`\tau_{w,g,s1,j}` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      \tau_{w,g,s1,j} = \begin{cases}
        \tau_{w,g,j} & ( \text{単層の場合} ) \\
        \frac{ 0.379 \cdot \rho_{w,g,s2f,j} \cdot \tau_{w,g,j} + \sqrt{ ( 0.379 \cdot \rho_{w,g,s2f,j} \cdot \tau_{w,g,j} )^2 - 4 \cdot ( 0.379 \cdot \rho_{w,g,s2f,j} - 1 ) \cdot \tau_{w,g,j} } }{ 2 } & ( \text{複層の場合} )
      \end{cases}
      \tag{23}
    \end{align*}

| ここで
|   :math:`\tau_{w,g,s1,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの透過率, -
|   :math:`\tau_{w,g,j}` : 境界 |j| の窓のガラス部分の日射透過率, -
|   :math:`\rho_{w,g,s2f,j}` : 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側）, -
| である。

境界 |j| の窓のガラス部分の日射透過率τ_(w,g,j)は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        \tau_{w,g,j} = \begin{cases}
          \frac{ \eta_{w,g,j} - ( 1 - \rho_{w,g,s1f,j} ) \cdot r_{R,w,g,j} }{ 1 - r_{R,w,g,j} } & ( \text{単層の場合} ) \\
          \frac{ \eta_{w,g,j} - ( 1 - \rho_{w,g,s1f,j} ) \cdot r_{R,w,g,j} }{ ( 1 - r_{R,w,g,j} ) - \rho_{w,g,s2f,j} \cdot r_{R,w,g,j} } & ( \text{複層の場合} )
      \end{cases}
      \tag{24}
    \end{align*}

| ここで
|   :math:`\tau_{w,g,j}` : 境界 |j| の窓のガラス部分の日射透過率, -
|   :math:`\eta_{w,g,j}` : 境界 |j| の窓のガラス部分の日射熱取得率, -
|   :math:`r_{R,w,g,j}` : 境界 |j| の窓のガラス部分の日射吸収量に対する室内側に放出される量の割合, -
|   :math:`\rho_{w,g,s1f,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側）, -
|   :math:`\rho_{w,g,s2f,j}` : 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側）, -
| である。

境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側） :math:`\rho_{w,g,s2f,j}` は次式により表される。

(複層のみ)

.. math::
    :nowrap:

    \begin{align*}
        \rho_{w,g,s2f,j} = 0.077
      \tag{25}
    \end{align*}

| ここで
|   :math:`\rho_{w,g,s2f,j}` : 境界 |j| の窓のガラス部分の室外側から2枚目の板ガラスの反射率（正面側）, -
| である。

境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側） :math:`\rho_{w,g,s1f,j}` は次式で表される。

.. math::
    :nowrap:

    \begin{align*}
        \rho_{w,g,s1f,j} = 0.923 \cdot t_j^2 - 1.846 \cdot t_j + 1
      \tag{26}
    \end{align*}

| ここで
|   :math:`\rho_{w,g,s1f,j}` : 境界 |j| の窓のガラス部分の室外側から1枚目の板ガラスの反射率（正面側）, -
|   :math:`t_j` : :math:`\rho_{w,g,s1f,j}` をベジェ曲線で表した時の0から1の値をとる媒介変数
| である。

:math:`\rho_{w,g,s1f,j}` をベジェ曲線で表した時の0から1の値をとる媒介変数 :math:`t_j` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        t_j = \frac{ -1.846 \cdot r_{R,w,g,j} + \sqrt{ ( 1.846 \cdot r_{R,w,g,j} )^2 + 4 \cdot ( 1 - 1.846 \cdot r_{R,w,g,j} ) \cdot \eta_{w,g,j} } }{ 2 \cdot ( 1 - 1.846 \cdot r_{R,w,g,j} ) } 
      \tag{27}
    \end{align*}

| ここで
|   :math:`t_j` : :math:`\rho_{w,g,s1f,j}` をベジェ曲線で表した時の0から1の値をとる媒介変数
|   :math:`\eta_{w,g,j}` : 境界 |j| の窓のガラス部分の日射熱取得率, -
|   :math:`r_{R,w,g,j}` : 境界 |j| の窓のガラス部分の日射吸収量に対する室内側に放出される量の割合, -
| である。

境界 |j| の窓のガラス部分の日射吸収量に対する室内側に放出される量の割合 :math:`r_{R,w,g,j}` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
      r_{R,w,g,j} = \begin{cases}
        \left( \frac{1}{2} \cdot ( \frac{ 1 }{ U_{w,g,j} } - R_{w,o,w} - R_{w,i,w} ) + R_{w,o,s} \right) \cdot U_{w,g,s,j} & ( \text{単層の場合} ) \\
        \left( \frac{1}{4} \cdot ( \frac{ 1 }{ U_{w,g,j} } - R_{w,o,w} - R_{w,i,w} - R_{w,air} ) + R_{w,o,s} \right) \cdot U_{w,g,s,j} & ( \text{複層の場合} )
      \end{cases}
      \tag{28}
    \end{align*}

| ここで
|   :math:`r_{R,w,g,j}` : 境界 |j| の窓のガラス部分の日射吸収量に対する室内側に放出される量の割合, -
|   :math:`U_{w,g,j}` : 境界 |j| の窓のガラス部分の熱損失係数（U値）, W / |m2| K
|   :math:`R_{w,o,w}` : 窓の室外側表面熱伝達抵抗（冬期条件）, |m2| K / W
|   :math:`R_{w,i,w}` : 窓の室内側表面熱伝達抵抗（冬期条件）, |m2| K / W
|   :math:`R_{w,air}` : 複層ガラスにおける窓の中空層の熱伝達抵抗, |m2| K / W
|   :math:`R_{w,o,s}` : 窓の室外側表面熱伝達抵抗（夏期条件）, |m2| K / W
|   :math:`U_{w,g,s,j}` : 境界 |j| の窓のガラス部分の熱損失係数（夏期条件）, W / |m2| K
| である。

複層ガラスにおける窓の中空層の熱伝達抵抗 :math:`R_{w,air}` は、0.003 |m2| K / W とする。

境界 |j| の窓のガラス部分の熱損失係数(夏期条件) :math:`U_{w,g,s,j}` は次式により表される。

.. math::
    :nowrap:
    
    \begin{align*}
        U_{w,g,s,j} = \frac{ 1 }{ \frac{ 1 }{ U_{w,g,j} } - R_{w,o,w} - R_{w,i,w} + R_{w,o,s} + R_{w,i,s} }
        \tag{29}
    \end{align*}

| ここで
|   :math:`U_{w,g,s,j}` : 境界 |j| の窓のガラス部分の熱損失係数(夏期条件), W / |m2| K
|   :math:`U_{w,g,j}` : 境界 |j| の窓のガラス部分の熱損失係数(U値), W / |m2| K
|   :math:`R_{w,o,w}` : 窓の室外側表面熱伝達抵抗(冬期条件), |m2| K / W
|   :math:`R_{w,i,w}` : 窓の室内側表面熱伝達抵抗(冬期条件), |m2| K / W
|   :math:`R_{w,o,s}` : 窓の室外側表面熱伝達抵抗(夏期条件), |m2| K / W
|   :math:`R_{w,i,s}` : 窓の室内側表面熱伝達抵抗(夏期条件), |m2| K / W
| である。

窓の表面熱伝達抵抗は次表の値とする。

.. list-table:: 表2 窓の表面熱伝達抵抗
    :header-rows: 1

    * - 項目
      - 記号
      - 熱伝達抵抗, |m2| K / W
    * - 窓の室外側表面熱伝達抵抗（冬期条件）
      - :math:`R_{w,o,w}`
      - 0.0415
    * - 窓の室内側表面熱伝達抵抗（冬期条件）
      - :math:`R_{w,i,w}`
      - 0.1228
    * - 窓の室外側表面熱伝達抵抗（夏期条件）
      - :math:`R_{w,o,s}`
      - 0.0756
    * - 窓の室内側表面熱伝達抵抗（夏期条件）
      - :math:`R_{w,i,s}`
      - 0.1317


境界 |j| の窓のガラス部分の日射熱取得率(η値) :math:`\eta_{w,g,j}` は次式により表される。

.. math::
    :nowrap:
    
    \begin{align*}
        \eta_{w,g,j} = \frac{ \eta_{w,j} }{ r_{A,w,g,j} }
        \tag{30}
    \end{align*}

| ここで
|   :math:`\eta_{w,g,j}` : 境界 |j| の窓のガラス部分の日射熱取得率, -
|   :math:`\eta_{w,j}` : 境界 |j| の窓の日射熱取得率(η値), -
|   :math:`r_{A,w,g,j}` : 境界 |j| の窓の面積に対するグレージングの面積の比, -
| である。

境界 |j| の窓のガラス部分の熱損失係数(U値) :math:`U_{w,g,j}` は次式により表される。

.. math::
    :nowrap:

    \begin{align*}
        U_{w,g,j} = \frac{ U_{w,j} - U_{w,f,j} \cdot ( 1 - r_{A,w,g,j} ) }{ r_{A,w,g,j} }
        \tag{31}
    \end{align*}

| ここで
|   :math:`U_{w,g,j}` : 境界 |j| の窓のガラス部分の熱損失係数(U値), W / |m2| K
|   :math:`U_{w,j}` : 境界 |j| の窓の熱損失係数(U値), W/m2K
|   :math:`U_{w,f,j}` : 境界 |j| の窓の建具部分の熱損失係数(U値), W / |m2| K
|   :math:`r_{A,w,g,j}` : 境界 |j| の窓の面積に対するグレージングの面積の比, -
| である。

境界 |j| の窓の面積に対するグレージングの面積の比 :math:`r_{A,w,g,j}` は指定するか、
不明な場合は建具(フレーム)材質の種類に応じて以下の値を用いる。

.. list-table:: 表3 窓の面積に対するグレージングの面積の比
    :header-rows: 1

    * - 建具（フレーム）材質
      - 窓の面積に対するグレージングの面積の比, -
    * - 樹脂製建具
      - 0.72
    * - 木製建具
      - 0.72
    * - 金属製建具
      - 0.8
    * - 木と金属の複合材料製建具
      - 0.8
    * - 樹脂と金属の複合材料製建具
      - 0.8

境界 |j| の窓の建具部分の熱損失係数(U値) :math:`U_{w,f,j}` は、建具（フレーム）材質が明らかな場合は次表によるものとし、不明な場合は次表のアルミ樹脂複合の値を用いることとする。

.. list-table:: 表4 窓の建具部分の熱損失係数(U値)
    :header-rows: 1

    * - 建具（フレーム）材質
      - 窓の建具部分の熱損失係数(U値), W / |m2| K
    * - 樹脂製建具
      - 2.2
    * - 木製建具
      - 2.2
    * - 金属製建具
      - 6.6
    * - 木と金属の複合材料製建具
      - 4.7
    * - 樹脂と金属の複合材料製建具
      - 4.7


 






