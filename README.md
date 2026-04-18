If you are familiar with Quantiacs https://quantiacs.com/, this is a Quantiacs strategy. <br>
<img width="1734" height="604" alt="{EACD8052-B131-40F0-80E6-20940D6BCCBC}" src="https://github.com/user-attachments/assets/dc1c35f4-32b9-470e-be65-21494e72d89d" />
Equity calculated by me vs Quantiacs' equity:
<img width="700" height="539" alt="{188A557A-405F-4FEF-84B4-37B902A3B807}" src="https://github.com/user-attachments/assets/7dc34e96-ccb0-404e-be31-25c41e9f8829" />
<br>
Designed and develop algo trading strategy on Quantiacs.com using TensorFlow (TF). The architecture is a differentiable, feedback-driven portfolio-optimization network (~1200 parameters) that refines initial daily weight predictions by simulating trading behaviour and optimizing the final equity and Sharpe ratio Two-stage, end-to-end differentiable trading system that learns portfolio allocation weights by optimizing final equity and Sharpe ratio using gradient-based training. <br>
A feed-forward neural network processes technical-indicator inputs for each asset (up to 800 S&P assets) and produces raw allocation signals, daily and for every asset from 2005 to current date. These signals are reshaped into an allocation matrix and normalized to form preliminary portfolio weights. The preliminary weights are fed into a differentiable time-evolving trading simulator implemented with tf.scan. The simulator incorporates metrics such as price series, slippage, position updates, transaction effects, inventory dynamics. It generates a sequence of portfolio states, including positions and cash flow. <br>
From the simulated state trajectory, the model computes several differentiable trading-behaviour metrics (KPIs), such as average price paid, average holding time, total traded quantity, turnover rate, holding duration, position-change magnitude. These KPIs summarize the consequences of the model’s own preliminary decisions, to represents the portfolio composition, and are concatenated with the original input features and fed into a second feed-forward network. <br>
This network generates the final portfolio daily allocation weights, now refined using feedback from simulated trading behaviour.
The entire pipeline, including both networks, the simulator, and the KPI computations, is differentiable.<br>
Using tf.GradientTape and Adam, the model directly maximizes:<br>
•	Final Equity<br>
•	Average Sharpe ratio, computed from the final simulated equity curve<br>
•	Enforce constraints on assets allocation bias and maximum asset allocation<br>
This trains the system not to predict returns, but to produce actionable daily assets’ allocation that perform well inside the simulator.<br>
Algo currently excluded from Quantiacs.com contests because still highly correlated with few other strategies with slightly higher Sharpe Ratio. The main limitations are the computational resources (10Gb Memory) allowed by Quantiacs.com, where my model can do only 5 iterations under 10 minutes. Several more iterations and features (technical KPIs) would be needed for the model to learn more original strategies.
