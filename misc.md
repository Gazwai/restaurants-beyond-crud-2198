<!-- Reviews -->

<!-- New Reviews -->

<h1>New review</h1>
<%= simple_form_for [@restaurant, @review] do |f| %>
  <%= f.input :content %>
  <%= f.submit "Submit review", class: "btn btn-primary" %>
<% end %>


<!-- Reviews Controller -->
class ReviewsController < ApplicationController
  before_action :set_restaurant, only: [:new, :create]

  def new
    @review = Review.new
  end

  def create
    @review = Review.new(review_params)
    @review.restaurant = @restaurant

    if @review.save
      redirect_to restaurant_path(@restaurant)
    else
      render :new, status: :unprocessable_entity
    end
  end

  def destroy
    @review = Review.find(params[:id])

    @review.destroy
    redirect_to restaurant_path(@review.restaurant), status: :see_other
  end

  private

  def set_restaurant
    @restaurant = Restaurant.find(params[:restaurant_id])
  end

  def review_params
    params.require(:review).permit(:content)
  end
end

<!-- top 5  -->

<div class="container mt-3">
  <div class="d-flex justify-content-between align-items-center">
    <h1>Restaurants</h1>
    <%= link_to new_restaurant_path, class: 'text-dark text-decoration-none' do %>
      <i class="fa-solid fa-plus"></i>
    <% end %>
  </div>
  <div class="cards">
    <% @restaurants.each do |restaurant| %>
      <%= render 'restaurants/card', restaurant: restaurant %>
      <p>RATING: <%= restaurant.rating %></p>
    <% end %>
  </div>
</div>
