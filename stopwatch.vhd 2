library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity stopwatch is
	    port (
	            clk      : in  std_logic;           -- Clock input (50 MHz)
	            reset    : in  std_logic;           -- Reset button
	            go       : in  std_logic;           -- Go button (start/stop)
	            seg_disp : out std_logic_vector(41 downto 0) -- 6 Seven-Segment Displays
		        );
end entity stopwatch;

architecture Behavioral of stopwatch is

	signal clk_count     : unsigned(19 downto 0) := (others => '0'); -- Count up to 500,000 (for hundredths)
	signal hundredths    : unsigned(6 downto 0) := (others => '0');
	signal seconds       : unsigned(5 downto 0) := (others => '0');
        signal minutes       : unsigned(5 downto 0) := (others => '0');
	signal running       : std_logic := '0'; -- State signal for running or stopped

	-- 7-segment display output for each digit (MM:SS.HH)
	signal seg_min_tens   : std_logic_vector(6 downto 0);
	signal seg_min_units  : std_logic_vector(6 downto 0);
	signal seg_sec_tens   : std_logic_vector(6 downto 0);
	signal seg_sec_units  : std_logic_vector(6 downto 0);
	signal seg_hund_tens  : std_logic_vector(6 downto 0);
	signal seg_hund_units : std_logic_vector(6 downto 0);

begin

	-- Main stopwatch process
	process (clk)
	begin
		if rising_edge(clk) then
			if reset = '1' then
				clk_count   <= (others => '0');
				hundredths  <= (others => '0');
				seconds     <= (others => '0');
				minutes     <= (others => '0');
				running     <= '0';
			elsif go = '1' then
				running <= '1';
			elsif go = '0' then
				running <= '0';
			end if;

			if running = '1' then
				if clk_count = 500000 - 1 then
					clk_count <= (others => '0');
					if hundredths = 99 then
						hundredths <= (others => '0');
						if seconds = 59 then
							seconds <= (others => '0');
							if minutes = 59 then
								minutes <= (others => '0');
							else
								minutes <= minutes + 1;
							end if;
						else
							seconds <= seconds + 1;
						end if;
					else
						hundredths <= hundredths + 1;
					end if;
				else
					clk_count <= clk_count + 1;
				end if;
			end if;
		end if;
	end process;

	-- Seven-segment display driver logic
	process (minutes, seconds, hundredths)
	begin
		-- Convert each digit to the seven-segment code for each display
		seg_min_tens   <= convert_to_7seg(minutes / 10); -- Minute tens
		seg_min_units  <= convert_to_7seg(minutes mod 10); -- Minute units

		seg_sec_tens   <= convert_to_7seg(seconds / 10); -- Second tens
		seg_sec_units  <= convert_to_7seg(seconds mod 10); -- Second units
		
		seg_hund_tens  <= convert_to_7seg(hundredths / 10); -- Hundredths tens
		seg_hund_units <= convert_to_7seg(hundredths mod 10); -- Hundredths units

		-- Combine all digits for output
		seg_disp <= seg_min_tens & seg_min_units & seg_sec_tens & seg_sec_units & seg_hund_tens & seg_hund_units;
	end process;

	-- Function to convert digit to seven-segment encoding
	function convert_to_7seg(input : unsigned(3 downto 0)) return std_logic_vector is
	begin
		case input is
			when "0000" => return "0111111"; -- 0
			when "0001" => return "0000110"; -- 1
			when "0010" => return "1011011"; -- 2
			when "0011" => return "1001111"; -- 3
			when "0100" => return "1100110"; -- 4
			when "0101" => return "1101101"; -- 5
			when "0110" => return "1111101"; -- 6
			when "0111" => return "0000111"; -- 7
			when "1000" => return "1111111"; -- 8
			when "1001" => return "1101111"; -- 9
			when others => return "0000000"; -- Deault (blank)
		end case;
	end function;
end architecture Behavioral;
