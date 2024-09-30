library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity Lab_02 is
	port (
		ADC_CLK_10 : in STD_LOGIC;
		MAX10_CLK1_50 : in STD_LOGIC;
		MAX10_CLK2_50 : in STD_LOGIC;
		KEY : in STD_LOGIC_VECTOR(1 downto 0);
		LEDR : out STD_LOGIC_VECTOR(9 downto 0);
	);
end;

architecture behavioral of Lab_02 is
	component stopwatch
		generic (
			
		);
		port (
			
		);
	end component;
begin
	uut_00 : stopwatch
		generic map (
			
		)
		port map (
			clk => MAX10_CLK1_50;
			reset => KEY(0);
			go => KEY(1);
			seg_disp => ;
		);

end architecture behavioral;
