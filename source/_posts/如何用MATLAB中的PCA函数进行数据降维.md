---
title: 如何用MATLAB中的PCA函数进行数据降维
tags: PCA 降维 MATLAB
abbrlink: 27c4bdef
date: 2019-10-26 22:22:48
categories:
---

<!--more-->

<p style="margin-left:0pt;"><span style="color:#333333;">coeff = pca(X)返回n×p数据矩阵的主成分系数。X的行对应于观察值</span><span style="color:#333333;">，</span><span style="color:#333333;">就是样本</span><span style="color:#333333;">，列对应于变量</span><span style="color:#333333;">，</span><span style="color:#333333;">也就是特征</span><span style="color:#333333;">。</span></p>

<p style="margin-left:0pt;"><span style="color:#333333;">系数矩阵是p×p。coeff的每一列包含一个主成分的系数，这些列按成分方差的降序排列。</span></p>

<p style="margin-left:0pt;"><span style="color:#333333;">默认情况下，pca以数据为中心，使用奇异值分解(SVD)算法。</span></p>

<p style="margin-left:0pt;"><span style="color:#333333;">一般用下面这个函数来进行获取投影矩阵Pro_Matrix。</span></p>

<p style="margin-left:0pt;"><span style="color:#333333;">获得投影矩阵后，通过下面这条语句得到降维后的数据。</span></p>

<pre class="has">
<code>Y=Pro_Matrix'*X;</code></pre>

<pre class="has">
<code>function [Pro_Matrix,Mean_Image]=my_pca(Train_SET,Eigen_NUM)
%输入：
%Train_SET：训练样本集，每列是一个样本，每行一类特征，Dim*Train_Num
%Eigen_NUM：投影维数

%输出：
%Pro_Matrix：投影矩阵
%Mean_Image：均值图像

[Dim,Train_Num]=size(Train_SET);

%当训练样本数大于样本维数时，直接分解协方差矩阵
if Dim&lt;=Train_Num  % 比如 100个特征 150列数据，数据量大于特征量  
    Mean_Image=mean(Train_SET,2);
    Train_SET=bsxfun(@minus,Train_SET,Mean_Image);
    R=Train_SET*Train_SET'/(Train_Num-1);
    
    [eig_vec,eig_val]=eig(R);
    eig_val=diag(eig_val);
    [~,ind]=sort(eig_val,'descend');
    W=eig_vec(:,ind);
    Pro_Matrix=W(:,1:Eigen_NUM);
    
else
    %构造小矩阵，计算其特征值和特征向量，然后映射到大矩阵
    Mean_Image=mean(Train_SET,2);  % 200个特征 100列数据，数据量明显不足
    Train_SET=bsxfun(@minus,Train_SET,Mean_Image);
    R=Train_SET'*Train_SET/(Train_Num-1);
    
    [eig_vec,eig_val]=eig(R);
    eig_val=diag(eig_val);
    [val,ind]=sort(eig_val,'descend');
    W=eig_vec(:,ind);
    Pro_Matrix=Train_SET*W(:,1:Eigen_NUM)*diag(val(1:Eigen_NUM).^(-1/2));
end

end
</code></pre>

<p style="margin-left:0pt;">有时候我们并不知道要降至多少维，但是有贡献率的要求，比如我希望降维之后的空间特征可以有90%的贡献率，那么可以用下面这个程序：</p>

<p style="margin-left:0pt;"> </p>

<p style="margin-left:0pt;"> </p>
