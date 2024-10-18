(function ($) {
	'use strict';

	/**
	 * All of the code for your public-facing JavaScript source
	 * should reside in this file.
	 *
	 * Note: It has been assumed you will write jQuery code here, so the
	 * $ function reference has been prepared for usage within the scope
	 * of this function.
	 *
	 * This enables you to define handlers, for when the DOM is ready:
	 *
	 * $(function() {
	 *
	 * });
	 *
	 * When the window is loaded:
	 *
	 * $( window ).load(function() {
	 *
	 * });
	 *
	 * ...and/or other possibilities.
	 *
	 * Ideally, it is not considered best practise to attach more than a
	 * single DOM-ready or window-load handler for a particular page.
	 * Although scripts in the WordPress core, Plugins and Themes may be
	 * practising this, we should strive to set a better example in our own work.
	 */

	jQuery(document).ready(function ($) {

		function pisol_prefetchRedirectUrl() {
			var product = [];
			var count = 0;
			$(".ajax_add_to_cart").each(function () {
				product[count] = $(this).data("product_id");
				count++;
			});
			$.ajax({
				url: pisol_dcw_setting.ajax_url,
				method: 'POST',
				dataType: 'json',
				data: {
					product_ids: product,
					action: "pisol_ajax_bulk_fetch_add_to_cart_redirect_url"
				}
			}).done(function (res) {
				window['bulk_redirect'] = res;
			});
		}
		if (pisol_dcw_setting != undefined && pisol_dcw_setting.ajax_url != undefined && pisol_dcw_setting.pre_fetch) {
			pisol_prefetchRedirectUrl();
		}

		var variation_obj = new variation()
		variation_obj.init();
		function variation() {
			this.is_variable = false;

			this.init = function () {
				this.variationChange();
				this.variationReset();
				this.addHidden();
				this.addLoadingIcon();
			}

			this.variationReset = function () {
				var parent = this;
				$(document).on('reset_data', "form.variations_form", function (event, data) {
					var button = jQuery(".pisol_single_buy_now", this);
					parent.statusBuyNow(button, false);
				});
			}

			this.variationChange = function () {
				var parent = this;
				$(document).on('show_variation', "form.variations_form", function (event, data) {
					var button = jQuery(".pisol_single_buy_now", this);
					if (data != undefined) {
						if (data.is_in_stock) {
							parent.statusBuyNow(button, true);
						} else {
							parent.statusBuyNow(button, false);
						}
					} else {
						parent.statusBuyNow(button, false);
					}
				});
			}

			this.statusBuyNow = function (button, status) {
				var $ = jQuery;
				if (status) {
					button.attr("disabled", false);
					button.removeClass('disabled');
				} else {
					button.attr("disabled", true);
					button.addClass('disabled');
				}
			}

			this.addHidden = function () {
				var $ = jQuery;
				$(document).on("click", ".pisol_single_buy_now", function (e) {
					/*
					$(this).after('<input type="hidden" name="pi_quick_checkout" value="true"/>');
					$(document).trigger('pisol_dtt_buy_now_clicked', [this]);
					$(this).off('click', function () { //was unbind()
						$(this).trigger('click');
					});
					*/
					$(this).off('click', function () { //was unbind()
						$(this).trigger('click');
					});
					/** this prevents ajax add to cart added by other plugins */
					$(document).trigger('pisol_dtt_buy_now_clicked', [this]);
					jQuery(document.body).off('submit', 'form.cart');
				});
			}

			this.addLoadingIcon = function () {
				$(document).on('pisol_dtt_buy_now_clicked', function (e, element) {
					jQuery(element).addClass(pisol_dcw_setting.buy_now_clicked_class);
				});

				$(document).on('click', '.pisol_buy_now_button', function (e) {
					jQuery(this).addClass(pisol_dcw_setting.buy_now_clicked_class);
				});
			}
		}
	});

})(jQuery);
