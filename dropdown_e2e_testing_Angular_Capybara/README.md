# Dropdown e2e testing with Angular and Capybara

## test that the select value is above the other options when open

```rb
		scenario %{test that the select value is above the other options when open} do
			select = first %{.f_o_r_m_my-dropdown-latch-dropdown-base}
			options = (capybara_result_to_array :target => (all %{.f_o_r_m_my-dropdown-latch-dropdown-base})      )
			options = options.slice(1,4)
			select.click
			value = select.style(%{top},%{height})
			value["top"]=    (numberParse :dimension =>value["top"]).to_f
			value["height"]= (numberParse :dimension =>value["height"]).to_f
			tops = {
				:values =>[value ]
			}
			options
			.each do |x|
				# execute_script %Q{
				# 	return Array.from(document.querySelectorAll(".f_o_r_m_my-dropdown-latch-dropdown-base")).slice(1)
				# 	.forEach((x,i)=>{
				# 		x.style["background-color"] = "red"
				# 	})
				# }
				value = x.style(%{top},%{height})
				value["top"]=    (numberParse :dimension =>value["top"] ).to_f
				value["height"]= (numberParse :dimension =>value["height"] ).to_f
				tops[:values].push(
					value
				)
			end

			tops[:values]
			.each_with_index.inject do |acc,x,i|

				p %{\\n}
				my_acc = acc
				.collect do |x|
					x[0]
				end
				p acc,my_acc,x
				p %{\\n}

				# make sure height of select and options are equal
					# make sure the select elements are stack one after another
				unless x[1]== 1
					expect(x[0]["height"]).to eq (my_acc.reverse[0]["height"])
					expect(x[0]["top"]).to be_within(2).of(my_acc.reverse[0]["height"] + my_acc.reverse[0]["top"])
				else
					expect(x[0]["height"]).to eq (acc[0]["height"])
					expect(x[0]["top"]).to be_within(2).of(acc[0]["height"] + acc[0]["top"])

				end
				#

				p acc,x # turns acc to array purshing values along
			end

		end
```

## test that when you click an option,select gets updated,dropdown closes

```rb
		scenario %{test that when you click an option,select gets updated,dropdown closes} do
			# select gets updated with the chosen option
			select = first %{.f_o_r_m_my-dropdown-latch-dropdown-base}
			select_text = select[:text]
			options = (capybara_result_to_array :target => (all %{.f_o_r_m_my-dropdown-latch-dropdown-base})      )
			options = options.slice(1,4)
			select.click
			item_to_click = {
				:element => options.sample
			}
			item_to_click[:element].click
			expect( item_to_click[:element][:text]).to eq(select[:text])
			#

			# dropdown closes
			options
			.each_with_index do |x,i|
				unless i == 0
					expect(x.style(%{top},%{height})).to eq(options[i-1].style(%{top},%{height}))
				end
			end
			#

			# update to the orgininal select al
			select.click
			item_to_click[:element].click
			expect( select_text).to eq(select[:text])
			#

			# dropdown closes
			options
			.each_with_index do |x,i|
				unless i == 0
					expect(x.style(%{top},%{height})).to eq(options[i-1].style(%{top},%{height}))
				end
			end
			#


		end
```