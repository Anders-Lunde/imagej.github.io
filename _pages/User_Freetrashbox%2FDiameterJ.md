{{Infobox
| software               = ImageJ 1.48 or newer (including ImageJ 2.XX) and Fiji
| name                   = DiameterJ
| author                  = {{Person|DiameterJ}}
| maintainer           = {{Person|DiameterJ}}
| filename              = ImageJ 1.48a to 2.XXX [http://imagej.net/_images/c/cc/DiameterJ_1-018.zip DiameterJ v1.018] 
Fiji any version [http://imagej.net/_images/6/65/DiameterJ_Fiji_-_1-018.zip DiameterJ v1.018]
| source                 = [https://github.com/NHotaling/DiameterJ Source Code] 
| released              = February  2015
| latest version      = August 5<sup>th</sup>, 2016
| status                  = v X.003 (first version released publicly)
| category               = [[:Category:Plugins|Plugins]] [[:Category:Analysis|Analysis]]
}}
'''DiameterJ'''<ref name="Hotaling">Hotaling NA, Bharti K, Kriel H, Simon Jr. CG. DiameterJ: A validated open source nanofiber diameter measurement tool. Biomaterials 2015;61:327–38. doi:10.1016/j.biomaterials.2015.05.015 http://www.sciencedirect.com/science/article/pii/S0142961215004652</ref>はフリーでオープンソースのImageJ、ImageJ 2、Fiji用プラグインです。National Institute of Standards and Technologyで開発されました。DiameterJは検証済みのナノファイバーの直径解析ツールです。DiameterJは画像を分析し、ナノファイバーまたはマイクロファイバーの繊維軸方向に沿った直径を見つけることができ、得られたデータのヒストグラムを作成します。一般的な統計情報、たとえば平均繊維径、繊維径の最頻値(mode)なども求められます。DiameterJには[http://bigwww.epfl.ch/demo/orientation/ OrientationJ]が付属しており<ref name="OrientationJ">R. Rezakhaniha, A. Agianniotis, J. T. C. Schrauwen, A. Griffa, D. Sage, C. V. C. Bouten, F. N. van de Vosse, M. Unser and N. Stergiopulos, Experimental investigation of collagen waviness and orientation in the arterial adventitia using confocal laser scanning microscopy, Biomechanics and modeling in mechanobiology, SpringerLink (DOI: 10.1007/s10237-011-0325-z)</ref>
、ImageJ/Fijiの"Analyze Particles"のような感じで繊維の方向を分析することができ、これにより孔径や孔に関する統計情報を得ることもできます。
__TOC__

=== <big>概要</big> ===
----
[[File:02b_Hotaling_Visual_Abstract.png|500px|thumb|right|alt=Alt|Overview of DiameterJ analysis flow - (''Top'') SEM image --> segmented image --> stylized Euclidean distance transform. (''Bottom'') A few of the graphs capable of being produced from data given by DiameterJ]] 
DiameterJ<ref name="Hotaling" />は2つのステップで画像解析します:

# 画像をバイナリイメージ（黒白ピクセルのみ）に分割します。
#*DiameterJの16の初期設定分割アルゴリズムが「Segment SRM」「Segment Mixed」プラグインで組み込まれます。ただしこれらのアルゴリズムはすべてのSEMイメージで使われるわけではありません。
#*もし、ユーザーが分割アルゴリズムに満足いかなかった場合でも（言い換えれば白黒イメージがオリジナル画像の正確な表現になっていない場合でも）、他の方法で分割された画像を解析できる場合もあります。
# 分割画像の解析
#*DiameterJの測定はすべて、初期設定ではピクセル数で表現されます。
#*DiameterJの正当性は、130以上の合成デジタル画像、および直径が分かっている繊維が写っている電子顕微鏡画像で確認されました。
#*:-  <b>10ピクセル以下の繊維がある場合、および最小繊維径の繊維の数が10%を超える場合、10%以上の誤差が生じます。</b>
#*:-  現在のDiameterJは、.tif, jpeg, png, .bmp, .gifの画像のみを解析できます。
#*:-  512ピクセルを超える繊維は解析できません。
あなたが仕事でDiameterJを使う場合には、参考資料の欄に[http://www. ここ]を示すか、または次の記述をしてください。

==== 出典/参照情報 ====
: Hotaling NA, Bharti K, Kriel H, Simon Jr. CG. DiameterJ: A validated open source nanofiber diameter measurement tool. Biomaterials 2015;61:327–38. doi:10.1016/j.biomaterials.2015.05.015.

http://www.sciencedirect.com/science/article/pii/S0142961215004652

=== ダウンロードリンク ===
: ImageJ 1.48以降:  [http://imagej.net/_images/c/cc/DiameterJ_1-018.zip DiameterJ v. 1.018 for ImageJ]
: Fiji最新版: [http://imagej.net/_images/6/65/DiameterJ_Fiji_-_1-018.zip DiameterJ v. 1.018 for Fiji]

=== <big>DiameterJの仕組み</big> ===
----
[[File:Figure_1.png|right|thumb|400px|alt=Alt|Diagram of DiameterJ code]] 
DiameterJの目標<ref name="Hotaling" />は、8ビットのSEM画像をどんな解像度であろうとも60秒以内にデスクトップコンピューターで解析することです。この下に、DiameterJのアルゴリズムがどのように繊維径を解析し、その他の骨組みがどのようになっているのかを示す図を挙げます。

==== Segmentation ====
SEM画像は、まずImageJ/Fijiが持つ様々な閾値化技術を駆使して、白黒分割を行います。Segment SRMとSegment Mixed pluginsは、他の分割アルゴリズムに自動的に組み込まれます。具体的には、我々は、SEM画像に写るナノファイバーの解析に適した[[Statistical Region Merging]]アルゴリズム<ref name="nock">R. Nock, F. Nielsen (2004), "Statistical Region Merging", IEEE Trans. Pattern Anal. Mach. Intell. 26 (11): 1452-1458</ref>を発見しました。これは我々が、Johannes Schindelinが開発した偉大な業績である[[Statistical Region Merging]]アルゴリズムが繊維直径を横切る混合繊維の解析を可能にし、深さ方向に存在する繊維の状態を再現することができることに気づいたためです。これに加えて、我々はOtsu<ref>Otsu N. A threshold selection method from gray-level histograms. Automatica 1975;11:23–7.</ref> , Huang<ref>Huang L-K, Wang M-JJ. Image thresholding by minimizing the measures of fuzziness. Pattern Recognit 1995;28:41–51. doi:10.1016/0031-3203(94)E0043-K.</ref>, Kittler (min error)<ref>Kittler J, Illingworth J. Minimum error thresholding. Pattern Recognit 1986;19:41–7. doi:10.1016/0031-3203(86)90030-0.</ref>, Doyle (percentile) <ref>Doyle, W (1962), "Operation useful for similarity-invariant pattern recognition", Journal of the Association for Computing Machinery 9: 259-267, doi:10.1145/321119.321123</ref>,Zack (triangle)<ref>Zack GW, Rogers WE, Latt SA (1977), "Automatic measurement of sister chromatid exchange frequency", J. Histochem. Cytochem. 25 (7): 741–53, PMID 70454</ref>らが開発した一般的な画像分割アルゴリズムも使用しました。

画像分割した後は、残されたノイズや形状上の特徴を、D'Amore<ref name="damore">D’Amore A, Stella JA, Wagner WR, Sacks MS. Characterization of the complete fiber network topology of planar fibrous tissues and scaffolds. Biomaterials 2010;31:5345–54. doi:10.1016/j.biomaterials.2010.03.052.</ref>とGonzalez<ref name="gonzalez">Gonzalez RC, Eddins SL. Digital Image Processing Using [[MATLAB]], 2nd ed. 2nd edition. S.I.: Gatesmark Publishing; 2001.</ref>が示した手順を基本に平滑化しました。簡単に言えば、ImageJのノイズ除去コマンドを、画像の変化が無くなるまで数回繰り返しています。そして、ImageJsの浸食コマンド、拡張コマンド、そして最終的には浸食コマンドを使い、繊維の境界を際立たせ、孤立したピクセルエリアを消去します。センターラインの決定の精度を良くするため、我々はLamらが開発した手法<ref name="lam">Lam L, Lee S-W, Suen CY. Thinning Methodologies-A Comprehensive Survey. IEEE Trans Pattern Anal Mach Intell 1992;14:869–85. doi:10.1109/34.161346.</ref>を利用した。

[[File:Segmentation.png|600px|alt=Alt|Original image--> Segmented image with no processing--> Segmented image after smoothing and noise removal]]

==== Super Pixel Diameter ====
画像の白黒化の後、白いピクセルの箇所は繊維を意味します。それぞれの画像について、繊維方向は仮に2種類計算されます。ひとつはZhangとSuenによる<ref name="zhang">Zhang TY, Suen CY. A Fast Parallel Algorithm for Thinning Digital Patterns. Commun ACM 1984;27:236–9. doi:10.1145/357994.358023.</ref>もので(ImageJのSkeletonizeコマンド)、もう一つはVoronoiのテッセレーション（平面充填）<ref name="okabe">Okabe A. Spatial Tessellations: Concepts and Applications of Voronoi Diagrams. 2 edition. Chichester ; New York: Wiley; 2000.</ref>です(ImageJのVoronoiコマンド)。繊維径を測定するアルゴリズムは、必ずしも新しい繊維を意味しない枝分かれに非常に敏感です。Voronoiアルゴリズムは黒いピクセルの塊の距離が最大となるよう設定するので、このような形のものに影響されません。それぞれの繊維の中心線の長さが平均化され、繊維が占める場所は平均厚みとVoronoiの中心線の長さで分けられます。2つの中心線の長さは、デジタル合成された画像の解析と、　結果によって平均化されます。


The two centerline lengths were averaged based on results from analyzing digital synthetic images and the finding that one method consistently overestimated fiber length while the other consistently underestimated fiber length.

The Super Pixel name was chosen because the fiber area, in pixels, was divided by the center-line lengths, in pixels; thus producing a unitless value that is equivalent to mean fiber diameter (fiber length x diameter = fiber area). This value is therefore a transformed pixel unit and is equivalent to the mean fiber diameter under the assumption that the fibers are just long rectangles when segmented into 2D shapes.   

After its initial calculation the diameter calculation was then further refined via intersection correction.  Intersection correction was done by taking the average length of the centerline and subtracting a radius value (obtained from first approximation of the diameter as determined without intersection correction) for each three-point intersection and a diameter value (obtained from first approximation) for each four-point intersection of the fibers. Intersections of each centerline were found using the algorithm developed by Arganda-Carreras et al.<ref>Arganda-Carreras I, Fernández-González R, Muñoz-Barrutia A, Ortiz-De-Solorzano C. 3D reconstruction of histological sections: Application to mammary gland tissue. Microsc Res Tech 2010;73:1019–29. doi:10.1002/jemt.20829</ref>.  

A new diameter was then calculated using the new corrected length and the total fiber area and this processes was looped until the diameter converged to 1/1000th of a pixel.  Additionally, the number of intersections was also saved and the intersection density (ID) was calculated for a 100px x 100px (10<sup>4</sup>) pixel area by dividing the total number of intersections by the total area (in pixels) of the image and multiplying by 10<sup>4</sup>:  <math>ID = {intersections_{Total} \over pixels_{total}}*10^4</math> .  The characteristic length (CL) of fibers was defined as mean length of fiber between intersections and was calculated by dividing the total centerline length by the number of intersections: <math>CL = {Fiber_{Length} \over Intersections_{Total}}</math>.  The center-line length was calculated as the average of the axial thinning and Voronoi tessellation center-lines.

==== Fiber Diameter Histogram ==== 
[[File:Radius_Histogram.png|thumb|right|alt=Alt|Radius histogram produced by DiameterJ]]To obtain the distribution of fiber diameters the segmented image was transformed with a Euclidian distance transformation algorithm <ref>F. Leymarie, M. D. Levine, in: CVGIP Image Understanding, vol. 55 (1992), pp 84-94 http://dx.doi.org/10.1016/1049-9660(92)90008-Q</ref> (Distance Map command in ImageJ).  This algorithm takes a fiber pixel and finds the distance to the nearest orthogonal mesh hole using the square root of the sum of the square of the vertical and horizontal distances to the hole and then transforms the fiber pixel to a grey scale value equal to that distance.  The resulting image is a grey scale image rather than black and white.  The center-line calculated by the axial thinning algorithm above is then overlaid on top of the distance transformed image.  At each intersection of the centerline the greyscale value is found and radii values within that range are subtracted out from the center-line.  The greyscale values under the remaining centerline are then obtained and multiplied by 2 to get the value of all diameters not in an intersection area.  The subsequent histogram of greyscale values is then found and placed in a .csv file along with the overall average, standard deviation, median and mode of all diameter values.  The sensitive centerline was chosen because it was the more discriminating option and thus, had a higher likelihood of eliminating pixels that had a higher value than the real radius value.

==== Mesh Hole Analysis ==== 
[[File:Pore_Outlines.png|thumb|right|alt=Alt|Mesh holes measured by DiameterJ]]Segmented pictures contain only black and white pixels; with black pixels representing background and white pixels representing fibers.  Black pixels were analyzed using the Analyze Particles command in ImageJ.  This algorithm essentially finds discrete clusters of black pixels, counts the number of pixels in each cluster and then reports their area.  Pixel units were selected for particle analysis as well as a circularity from 0.00-1.00, the option to exclude clusters that touch the edge was also chosen.  The subsequent particle analysis was then saved to later produce a mesh hole histogram, mean mesh hole area (produced by averaging all cluster areas), and percent mesh hole (produced by taking the total number of black pixels and dividing it by the total image resolution).

==== Fiber Orientation ====
Fiber orientation was determined using a well-established plug-in for ImageJ called [http://bigwww.epfl.ch/demo/orientation/ OrientationJ]<ref name="OrientationJ" />.  To determine fiber orientation an axial thinning algorithm was used and then the centerline was enlarged by 2 pixels (using the Enlarge command in ImageJ) to ensure accurate measure of the line.  Within OrientationJ a Fourier gradient was used with a gaussian window of 7 pixels.  The subsequent frequency histogram of fiber orientation was then saved as an image.  OrientationJ limits access to the raw data for this histogram; thus, if the user desires the raw data they must use the "OrientationJ Distribution" plugin.  In the Fiji version of DiameterJ the [[Directionality]]<ref name="Directionality"> Liu. Scale space approach to directional analysis of images. Appl. Opt. (1991) vol. 30 (11) pp. 1369-1373</ref> plugin, written by Jean-Yves Tinevez, can also be used to obtain the raw distribution data.  This approach is considerably slower and less elegant (pop-up windows remain open) than OrientationJ but for now it is the only way to obtain the raw orientation data in batch.

=== <big>DiameterJの使い方</big> ===
----
====<big>DiameterJトレーニングモジュール</big>====

詳しい使い方を知るには、DiameterJユーザーのために開発された[https://sites.google.com/site/diameterj/ Learn DiameterJ]を御使用ください。トレーニングは主に7つの科目からできています:

# 初期設定/トレーニング/DiameterJの使い方
#* DiameterJのインストール方法概要
#* DiameterJのソフトウェア内容、操作法を解説するトレーニング文書
#* 単位をピクセルから長さに変える方法
#* クイズ形式で、トレーニング内容を確実にします
# プレテスト
#* 解析目的であれば、必ずしもこのトレーニングは必要ありません。
#* ただし、あなたが受けたプレテストの結果の公表に同意頂ければ、我々は感謝します。
# Training 1 -  切り抜きと領域の区分け
#* DiameterJで解析するための画像の切り抜き
#* DiameterJを使って領域を区分けする方法
#* いくつかの選択肢の中から、最もよい分割結果を選ぶ方法
# Training 2 - 手動操作による領域の区分け
#* このトレーニングで、自動区分けされた結果を効率的に手修正する方法を説明します。
# Training 3 - 繊維径の評価 
#* 平均繊維径を決定します。
#* 繊維径のヒストグラムの識別と使用法
#* 複数ピークを持つ分布の識別
#* 繊維分析結果を完了させるタイミング
# Training 4 - その他の計測に関する注意
#* 構造を表すその他の数値をDiameterJで求める方法
# 事後テスト
#* 解析目的であれば、必ずしもこのトレーニングは必要ありません。
#* ただし、あなたが受けたプレテストの結果の公表に同意頂ければ、我々は感謝します。

トレーニングのために[https://sites.google.com/site/diameterj/ Learn DiameterJ] にアクセスして下さい!

=== <big>DiameterJ Output</big> ===
----
{| class="wikitable collapsible collapsed" style="vertical-align:top; width:90%; margin: 1em auto 1em auto;"
|- id="section link anchor name" style="text-align:center" valign="top"
| colspan=2 | <h4> <big> Summaries Folder </big> </h4>
|- valign="top"
! scope="col" width="50%" | '''"File Source Name"_Total Summary.csv'''
! scope="col" width="50%" | '''"File Source Name"_Total Summary.csv Continued'''
|- valign="top"
|
#Super Pixel: スーパーピクセルの決定から計算された平均繊維径。計算方法の詳しいアルゴリズムは上で解説しています。
#Histogram Mean: 繊維径の分布にガウス曲線を当てはめて求めた、曲線の平均値
#Histogram SD: 当てはめたガウス曲線から計算した繊維径ヒストグラムの標準偏差。
#Histogram Mode: ヒストグラムの最頻値。
#Histogram Median: 繊維径の中央値。
#Histogram Min. Diam.: 測定された最小繊維径。
#Histogram Max. Diam.: 測定された最大繊維径。
#Histogram Integrated Density: 繊維長と平均繊維半径の積算値。
#Histogram Raw Integrated Density: 画像全体、または選択範囲の全てのピクセルの半径の合計。
#Diameter Skewness: 分布の歪度。平均の3次モーメント
#Diameter Kurtosis: 分布の尖度。平均の4次モーメント
|
#Fiber Length: 繊維長。2値化画像に含まれる繊維の中央線の合計長さ。
#Mean Pore Area: 平均開孔面積。（穴として数えられた黒色ピクセルの数）÷（画像中の穴の数）
#Pore Area SD: 測定された全ての穴の面積の標準偏差。
#Min. Pore Area: 測定された最小の穴面積。
#Max Pore Area: 測定された最大の穴面積。
#Percent Porosity: 開孔率。（黒色ピクセルの合計数）÷（画像中のピクセルの合計数）
#Intersection Density (100x100px): 100x100pxに含まれる繊維交差点の数密度。（繊維が重なった箇所の数）×10000÷（画像の総ピクセル数）
#Characteristic Length: （繊維中央線の合計長さ）÷（繊維交差点の数）
|- style="text-align:center" valign="top"
| colspan=2 | <h4> <big>Histograms Folder</big> </h4>
|- valign="top"
!'''"File Source Name"_Pore Data.csv'''
!'''"File Source Name"_Histogram.csv'''
|- valign="top"
|
#Slice: Image name
#Count: Total number of pores found in image that are not touching the side
#Area: Total number of black pixels in an image not in groups touching the sides
#StdDev: 0 (individual pore measurements so no SD
#% Area: (Total number of black pixels) / (Total pixels in an image)
#Major: The length of the primary axis of the best fitting ellipse for each pore. 
#Minor: The length of the secondary axis of the best fitting ellipse for each pore
#Angle: The angle between the primary axis and a line parallel to the X-axis of the image
#Circ.: 4π × [Area] / [Perimeter^2] with a value of 1.0 indicating a perfect circle. As the value approaches 0.0, it indicates an increasingly elongated shape :::#Values may not be valid for very small particles. Uses the heading Circ.
#Skew: The third order moment about the mean – NaN because only one pore
#Kurt: The fourth order moment about the mean – NaN because only one pore
#AR:  The aspect ratio of the particle’s fitted ellipse, i.e., [Major Axis] / [Minor Axis]
#Round:  4 × [Area] / (π × [Major axis]2) or the inverse of Aspect Ratio. 
#Solidity: [Area] / [Convex area]
|
#Radius Value:  Radius length (in pixels)
#Radius Count: Number of times the radius value occurred in the image. Also known as the frequency of occurrence and can also be interpreted as the length of fiber in an image that has a given radius
#*The radius or diameter histogram is constructed from the radius value (x-axis) and radius count (frequency of occurrence on y-axis)
|- valign="top"
!'''"File Source Name"_Intersection Coordinates.txt'''
!'''"File Source Name"_Radius Histogram.tif'''
|- valign="top"
|
#Column 1 – Grey scale value at intersection 
#Column 2 – x coordinate of intersection 
#*Upper left hand corner is 0,0
#Column 3 – y coordinate of intersection 
#*Upper left hand corner is 0,0
|
#Image of the histogram of all fiber radii in the image
|- style="text-align:center" valign="top"
| colspan=2 | <h4> <big>Diameter Analysis Images Folder</big> </h4>
|- valign="top"
!'''"File Source Name"_Compare.png'''
!'''"File Source Name"_Orientation.tif'''
|- valign="top"
|
A montage image with four images in it
# Top Left Image - Original segmented image
# Top right Image- Image of the centerline as determined by the Voronoi tessellation algorithm
# Bottom Left Image - Image of all centerlines counted in the histogram overlaid on the Euclidean distance transformed of the fibers fibers.
#*Yellow lines are the locations where radii were counted
#*Fibers are in greyscale as transformed by the Euclidean distance transform
# Bottom Right Image - Pores found and analyzed by Diameterj
|
#An image with the frequency of orientation of the centerline of all fibers. This is an output of OrientationJ and was not coded by any developers from DiameterJ
|}

=== <big>Limitations</big> ===
----
:#Due to mathematical limitations fibers that are smaller than 10px or greater than 10% of the smallest dimension of the image produce errors that are above 10% and thus deemed as too high for accurate measurement. This is because at small diameters (less than 10px) a single pixel error can cause 10% error in measurement.  While with large fibers (greater than 10% of the smallest dimension of the image) low sampling and edge effects of fibers become a dominant factor in diameter distribution. This limitation can be overcome by altering your magnification when taking the image to make fibers smaller/larger as appropriate.
:#Due to algorithmic reasons fibers that have a diameter larger than 512 px cannot be analyzed by DiameterJ.  In general, this is not a concern if the researcher is taking images that do not violate the first limitation because an image would have to be over 5120 px on its smallest dimension for this limitation to affect them. However, with stitched images and with ultra high image resolutions (36MP or greater) this could prove to be a limiting factor.  Work is underway to convert the Euclidean Distance Transform to a 16-bit algorithm that can analyze images with fibers that are 131,072 px or less in diameter.  
:#Images that have many fibers that entwine or that touch and run parallel to each other for long distances in an image can produce errant peaks in DiameterJ. This is usually due to segmentation of these images showing one larger fiber instead of many smaller fibers bundled. Additionally, images where partial fibers run along the edge of the image can cause DiameterJ to show fiber diameters that are smaller than the actual fiber.   Thus, a "common sense" check on all images must be performed and while taking images the researcher must be careful to select areas where fibers do not overlap and run parallel considerably or run along the edge of the image. 
:#An integral part of analysis for DiameterJ is image segmentation.  However, how an image is segmented into foreground or background can drastically alter the outputs produced by DiameterJ listed below:  
:#::* Normalized Orientation Index
:#::* Mean Mesh Hole Size
:#::* Percent Porocity
:#::* Intersection Density
:#::* Characteristic Fiber Length 
:#::* Fiber Orientation Histogram
:#:This is because the more fibers that are segmented out of an image the larger the pore size, the lower the intersection density and the higher the characteristic fiber length between intersections.  Additionally, fiber orientation can be shifted because the orientation of underlying fibers can be different than the fibers that are closer to the top layer.  Thus, while the algorithms analyzing these metrics are able to produce highly accurate results on calibration images; on real segmented images the analysis is entirely dependent on segmentation algorithm used.  While, these algorithms were incorporated into DiameterJ to provide a reference way to calculate these values we caution that unless identical segmentation algorithms and image capture settings are used, results from these algorithms are not comparable between images.  
:#::*<small>Fiber diameter is relatively invariant to segmentation because good image segmentation leads to having no partial fibers, only less fibers being included in the segmentation.  Thus, while different segmentation algorithms may produce increased or decreased fiber sampling the measure of fiber diameters within each segmentation is accurate.  Developing a segmentation algorithm that does not have this problem across any image is currently a topic of heavy research in the image analysis field.</small>
:#When analyzing the images with multiple distinct fiber diameters it has been found that DiameterJ under represented the prevalence of larger diameter fibers as compared to smaller fibers.  This bias leads to weighted averages being inaccurate and for global averages of fiber diameters to be biased toward smaller fiber diameters.  The bias is due to intersection correction/subtraction and the increased probability of larger fibers having more intersections due to their increased size.  This limitation can be partially overcome by directional intersection correction of fiber intersection, which is currently under development.  However, this limitation is inherent to all two-dimensional image analysis techniques and cannot be completely eliminated, making weighted averages of fiber diameter slightly inaccurate no matter what.  
:#Distinguishing between fiber diameters that are closer in diameter than 3 pixels often creates resolution difficulties for peak fitting and the prevalence of each fiber diameter can be skewed.  This has to do with inherent limitations due to pixel binning of diagonal lines in Euclidean Distance Transforms. This limitation can be overcome by increasing magnification of your image so that fibers with different diameters have more than 3 pixels of difference between them.

=== <big>Installation</big> ===
----
If you installed imageJ before the end of 2013 you should uninstall your current version of ImageJ ('''DO NOT UPDATE''') and reinstall ImageJ 1.48 or newer. 
:*Before uninstall be sure to copy all of your old plugins into a separate folder as these will be removed when you uninstall your old version of ImageJ.  
:*We recommend ImageJ over Fiji if you have no experience with either software because it is simpler to use.  

{| class="wikitable collapsible collapsed" style="vertical-align:top; width:90%; margin: 1em auto 1em auto;"
|- id="section link anchor name" style="text-align:center" valign="top"
| colspan=3 | <big> '''Download and install [http://rsb.info.nih.gov/ij/download.html ImageJ 1.48] or newer or [[Downloads|Fiji]] (any version)''' </big>
|- valign="top"
! scope="col" width="33%" | '''Windows'''
! scope="col" width="33%" | '''OSX'''
! scope="col" width="33%" | '''Linux'''
|- valign="top"
|
#Download and unzip the DiameterJ files (Find in "Source" above) and move or copy the three folders into the plugins folder of ImageJ.  
#That should be in directory:
#:"C:\Program Files\ImageJ\plugins"
#::Or 
#:"C:\Program Files (x86)\ImageJ\plugins"
#:*DiameterJ will work with x86 (32-bit) or x64 (64-bit) versions of Java/ImageJ
#Restart ImageJ 
|
#Follow instructions [http://rsb.info.nih.gov/ij/docs/install/osx.html#dandd Here] or [[MacOSX tips|Here]] for installation of ImageJ/Fiji on OSX 
|
#Download and unzip the DiameterJ files (Find in "File" in the Info box above) 
#Move or copy the three folders into the plugins folder of the directory where you have placed ImageJ.  
#:*DiameterJ will work with x86 (32-bit) or x64 (64-bit) versions of Java/ImageJ
#Restart ImageJ
|}

== FAQs ==
:# Q:  When running either segmentation algorithm an error occurs that says either "Unrecognized command: "Auto Threshold"" or "Unrecognized command: "Auto Threshold..""
:#:*  A: ''You have installed the wrong version of DiameterJ into your ImageJ/Fiji release.  Fiji gives the "Auto Threshold.." error and it means you installed ImageJ's plugin into Fiji. ImageJ gives the "Auto Threshold" error and it means you've installed Fiji's software version into ImageJ0. Please download and install the correct version of DiameterJ for the piece of software you are using.''
:# Q:  When I start ImageJ or Fiji for the first time after installing DiameterJ I get "Plugin configuration error: C:\...  Duplicate command: "XXXX" (already in "YYYY")
:#:  <small> Where "..." is the directory where your plugin is located, "XXXX" is the name of the plugin and "YYYY" is the name of the directory where that plugin is already.</small>
:#:*  A: ''You have duplicate plugins! Go to the file where you unzipped DiameterJ and its other plugins open the "DiameterJ", "OrientationJ", or "Analyze Skeleton 2D - 3D" folder" and delete the file named XXXX''
:# Q:  When I run DiameterJ an error occurs that says "Unrecognized Command: "Skeleton Intersections"" or "Unrecognized Command: "OrientationJ"" or "Unrecognized Command: "Analyze Skeleton (2D/3D)"" or "Unrecognized Command: "Statistical Region Merging""
:#:*  A: ''During installation one or more of the plugins that is needed for DiameterJ was missed.  Please go back to the zip file that you downloaded for DiameterJ and copy all files into the plugins folder of ImageJ/Fiji''
:# Q:  When I run DiameterJ an error occurs that says "Unrecognized Command: AAAA" where AAAA is any command not listed above.
:#:*  A: ''You are probably using a version of ImageJ that was updated to v. 1.48 or newer and did not create a fresh install of ImageJ.  DiameterJ uses several plugins/scripts that ImageJ does not include in its updates, they ONLY include them in fresh installs.  Thus you must uninstall ImageJ and reinstall a new version 1.48 or newer.  Please remember save a copy of any plugins you added to ImageJ before uninstalling it, these will be lost during the uninstall process unless you save a copy in another location on your computer.''
:# Q:  When I run DiameterJ an error appears in the log that says "Error there are no fibers in  "BBBB".tif  to analyze".
:#:*  A: ''The images that are in the folder that you have selected are not binary (black and white) images or the image is completely black.  Please segment your image before trying to analyze it with DiameterJ and then move the segmented image into a separate folder with only black and white images in it.''
:# Q: No error occurs but when I ask the Segment XXX or DiameterJ to analyze a folder that I have images in, no output is produced by Segment XXX/DiameterJ.
:#:*  A: ''The image is probably not a .tif file.  For now DiameterJ only analyzes .tif images. Please save your images as .tif files and then analyze with DiameterJ''
:# Q: DiameterJ keeps giving me an error on a file and won't continue on to the next file
:#:*  A: ''Unfortunately DiameterJ goes serially through files and isn't capable of skipping a file with an error.  Simply remove the file that is giving the error from the folder you wish to analyze and rerun DiameterJ.''
:# Q: None of the images I am analyzing are segmenting well with your algorithms, why not?
:#:*  A: ''The algorithms included by default with DiameterJ rely heavily on uniformity of fiber color and/or a dark background.  Below are four good examples and four examples that work poorly for image segmentation with the default algorithms.  Keep in mind there are many more segmentation algorithms than I have included with DiameterJ in both Fiji and ImageJ.  See the [[DiameterJ#Complementary Tools |Complementary Tools]] or [[DiameterJ#Image Segmentation |Image Segmentation]] sections of this work for a few of the options available.[[File:Good_vs_Bad_Seg.PNG|750px|center|alt=Alt|Example images that segment well and that do not segment well with DiameterJ's default segmentation algorithms]]

== Complementary Tools ==
DiameterJ<ref name="Hotaling" /> works with several plugins of ImageJ/Fiji.  First and foremost [http://bigwww.epfl.ch/demo/orientation/ OrientationJ]. Also, other great tools that we have incorporated are [[AnalyzeSkeleton]] function from Ignacio Arganda-Carreras, as well as [http://jvsmicroscope.uta.fi/?q=skeleton_intersections Skeleton Intersections] from Gabriel Landini's morphology plugin.   We'd also like to give a big thank you to the [[Statistical Region Merging]] algorithm which makes our segmentations work a lot better.  Speaking of which, we include a lot of segmentation algorithms that we aren't nearly talented enough to of developed ourselves. In particular those techniques outlined above in the [[DiameterJ#Segmentation |Segmentation]] section of this article and can be found in the links below.

We'd also like to say that DiameterJ plays nicely with the output from any other segmentation algorithm that produces a binary image.  Fiji has a ton of [[Segmentation]] algorithms that are great for different types of images.  Additionally, many plugins have been created for use in ImageJ or Fiji: [http://ij-plugins.sourceforge.net/plugins/segmentation/ IJ Plugins] [[Auto Threshold]] and [[Auto Local Threshold]].  If our defaults don't work then try any/all of these.  

Finally, we'd like to encourage everyone to do peak fitting of the diameter histograms that DiameterJ produces.  To do this any peak fitting tool can be used. A free resource for Windows is [http://fityk.nieto.pl/ Fityk] however, we don't recommend any software in particular.

== Future Development ==
:# Reducing bias of the Histogram intersection correction by directionally subtracting fiber intersections rather than blanket subtraction within a given intersection radius
:# incorporation of Gaussian peak fitting algorithms within DiameterJ itself.  
:#Currently we are working on a native JAVA application on DiameterJ.  This will not fundamentally change the function of DiameterJ however, it will make it faster, look cleaner, and should solve continuity issues
:# 16-bit Euclidean distance transform calculator
:# Enable DiameterJ to skip to the next file if an error occurs during analysis

<big>Completed Goals</big>
:# Combine the three different releases of DiameterJ into a single release for all versions of ImageJ/Fiji
:# Easier to use GUI (any GUI at all) with more options for what the outputs of DiameterJ are and what types of analysis it performs.
:# Make DiameterJ compatible with images other than .tif files

Help is welcome in any/all of these improvements!

== References ==
{{reflist}}

== External Links ==
* [http://bigwww.epfl.ch/demo/orientation/ OrientationJ]
* [http://www.nist.gov/ NIST]

[[Category:Plugins]]
[[Category:Analysis]]