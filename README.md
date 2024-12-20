# xtw_project
# Q1
runge = @(x) 1 ./ (1 + 25 * x.^2);

interval = [-1, 1];

n_values = [10, 20];

x_dense = linspace(interval(1), interval(2), 500);

f_x = runge(x_dense);

figure;

for i = 1:length(n_values)
    n = n_values(i);
    

  
    x_nodes = linspace(interval(1), interval(2), n);
    y_nodes = runge(x_nodes);
    
    % 差商表
    div_diff = zeros(n, n+1);  
    div_diff(:, 1) = x_nodes(:);  % col_1
    div_diff(:, 2) = y_nodes(:);  % col_2
    
    % 计算分差商
    for j = 3:n+1
        for k = j-1:n
            div_diff(k, j) = (div_diff(k, j-1) - div_diff(k-1, j-1)) / (div_diff(k, 1) - div_diff(k-j+2, 1));
        end
    end
    
    % 构造插值多项式
    N_interp = zeros(size(x_dense));
    for j = n:-1:1
        N_interp = div_diff(j, j+1) + (x_dense - x_nodes(j)) .* N_interp;
    end
    
    subplot(1, 2, i);
    plot(x_dense, f_x, 'b-', 'LineWidth', 1.5); hold on; % 原函数
    plot(x_dense, N_interp, 'r--', 'LineWidth', 1.5);       % 插值函数
    plot(x_nodes, y_nodes, 'ko', 'MarkerFaceColor', 'r');   % 节点
    hold off;
    
    title(['Newton Interpolation with n = ', num2str(n)]);
    legend('Runge Function $f(x)$', 'Interpolation', 'Nodes', 'Interpreter', 'latex');
    xlabel('x');
    ylabel('y');
    grid on;
end

sgtitle('Runge Function Interpolation based on Newton Method');
