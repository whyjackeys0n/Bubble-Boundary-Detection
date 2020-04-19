%% 清空环境变量
clear
clc

%% 读取图片
data = double(imread('10111.tif'));
[x,~] = size(data);
for i = 1:x
    if min(data(i,:))==0
        want(i) = 1;
    else
        want(i) = nan;
    end
end
data = data(want==1,:);
[x,y] = size(data);

%% 识别原点
bound_o = find(data(max(x),:)==0);
oyjc = round((max(bound_o)+min(bound_o))/2);
data_left = data(:,1:oyjc);
data_right = data(:,oyjc+1:y);

%% 识别边界点
for i = 1:x
    bound_left(i) = (oyjc - sum(data_left(i,:)==0))/length(bound_o);
end

for i = 1:x
    bound_right(i) = (oyjc + sum(data_right(i,:)==0) + 1)/length(bound_o);
end  

%% 边界绘图
figure(1)
mid = (x:-1:1)/length(bound_o);
plot(bound_left,mid,bound_right,mid,[bound_left(1),bound_right(1)],[x/length(bound_o),x/length(bound_o)],'color','b');
p1 = polyfit(mid,(oyjc+1)/length(bound_o)-bound_left,10); 

%% 拟合边界
yi = polyval(p1,mid); 
figure(2)
plot(mid,yi,mid,(oyjc+1)/length(bound_o)-bound_left,'.');

%% 体积
p11 = conv(p1,p1);
q = polyint(p11);
V = diff(polyval(q,[0 mid(1)])) * pi;

%% 表面积
dp1 = polyder(p1);
syms x
f3 = @(x) (p1(1)*x.^10 + p1(2)*x.^9 + p1(3)*x.^8 + p1(4)*x.^7 + p1(5)*x.^6 + p1(6)*x.^5 + p1(7)*x.^4 + p1(8)*x.^3 + p1(9)*x.^2 + p1(10)*x + p1(11)).*...
    sqrt(1 + (dp1(1)*x.^9 + dp1(2)*x.^8 + dp1(3)*x.^7 + dp1(4)*x.^6 + dp1(5)*x.^5 + dp1(6)*x.^4 + + dp1(7)*x.^3 + dp1(8)*x.^2 + dp1(9)*x + dp1(10)).^2);
S = 2 * pi * quad(f3,0,mid(1));

% mas实际1mm代表的像素点
mas = 320; 
chris = length(bound_o);
happy = mas / chris;
V = V / happy^3;
S = S / happy^2;
